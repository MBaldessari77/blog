---
title: "Safer AES 256 cryptography"
layout: post
date: 2017-10-06
categories: tip security code-snippet net c#
---

# Introduction

As increasing security and privacy requirements, is important to preserve __confidential informations__. For a developer can be normal to store and somewhat query those type of information from Database. A simple solution is to decrypt/encrypt sensible data, not choosing only a strong encryption algorithm and password, but also generate and use a different pseudo-casual IV (_Initialization Vector_) before each encrypting. The side effect of this choice is an increased CPU work and more encrypted data occupation, but this choice is best for store confidential data, specifically when information to encrypt assume only a few types of values (_find correlation will result much difficult and probably will become an intractable problem_).

# Pratically

## Code-snippets

### Class - Aes256Chiper

```csharp
	/// <see ref="https://msdn.microsoft.com/en-US/library/system.security.cryptography.aescryptoserviceprovider(v=vs.90).aspx"/>
	/// <see ref="http://stackoverflow.com/questions/8041451/good-aes-initialization-vector-practice"/>
	public class Aes256Chiper
	{
		const int KeyLength = 256 / 8;  //32 byte

		public string EncriptBase64(Guid key1, Guid key2, string input)
		{
			//Generate a different IV for each calls
			using (var stream = new MemoryStream())
			using (var aes = Aes.Create())
			{
				byte[] iv = aes.IV;
				stream.Write(BitConverter.GetBytes(iv.Length), 0, sizeof(int));
				stream.Write(iv, 0, iv.Length);
				var key = new byte[KeyLength];
				Buffer.BlockCopy(key1.ToByteArray(), 0, key, 0, KeyLength / 2);
				Buffer.BlockCopy(key2.ToByteArray(), 0, key, KeyLength / 2, KeyLength / 2);
				byte[] encripted = EncryptStringToBytesAes(input, key, iv);
				stream.Write(encripted, 0, encripted.Length);
				return Convert.ToBase64String(stream.ToArray());
			}
		}

		public string DecriptBase64(Guid key1, Guid key2, string input)
		{
			byte[] buffer = Convert.FromBase64String(input);
			using (var stream = new MemoryStream(buffer))
			{
				var ivlengthbuffer = new byte[sizeof(int)];
				stream.Read(ivlengthbuffer, 0, sizeof(int));
				int ivlength = BitConverter.ToInt32(ivlengthbuffer, 0);
				var iv = new byte[ivlength];
				stream.Read(iv, 0, ivlength);
				var key = new byte[KeyLength];
				Buffer.BlockCopy(key1.ToByteArray(), 0, key, 0, KeyLength / 2);
				Buffer.BlockCopy(key2.ToByteArray(), 0, key, KeyLength / 2, KeyLength / 2);
				var encripted = new byte[buffer.Length - sizeof(int) - ivlength];
				stream.Read(encripted, 0, buffer.Length - sizeof(int) - ivlength);
				return DecryptStringFromBytesAes(encripted, key, iv);
			}
		}

		static byte[] EncryptStringToBytesAes(string plainText, byte[] key, byte[] iv)
		{
			// Check arguments.
			if (plainText == null || plainText.Length <= 0)
				throw new ArgumentNullException(nameof(plainText));
			if (key == null || key.Length <= 0)
				throw new ArgumentNullException(nameof(key));
			if (iv == null || iv.Length <= 0)
				throw new ArgumentNullException(nameof(iv));
			byte[] encrypted;
			// Create an AesCryptoServiceProvider object
			// with the specified key and IV.
			using (var aes = Aes.Create())
			{
				aes.Key = key;
				aes.IV = iv;

				// Create a decrytor to perform the stream transform.
				ICryptoTransform encryptor = aes.CreateEncryptor(aes.Key, aes.IV);

				// Create the streams used for encryption.
				using (var msEncrypt = new MemoryStream())
				using (var csEncrypt = new CryptoStream(msEncrypt, encryptor, CryptoStreamMode.Write))
				{
					using (var swEncrypt = new StreamWriter(csEncrypt))
						//Write all data to the stream.
						swEncrypt.Write(plainText);
					encrypted = msEncrypt.ToArray();
				}
			}

			// Return the encrypted bytes from the memory stream.
			return encrypted;
		}

		static string DecryptStringFromBytesAes(byte[] cipherText, byte[] key, byte[] iv)
		{
			// Check arguments.
			if (cipherText == null || cipherText.Length <= 0)
				throw new ArgumentNullException(nameof(cipherText));
			if (key == null || key.Length <= 0)
				throw new ArgumentNullException(nameof(key));
			if (iv == null || iv.Length <= 0)
				throw new ArgumentNullException(nameof(iv));

			// Declare the string used to hold
			// the decrypted text.
			string plaintext;

			// Create an AesCryptoServiceProvider object
			// with the specified key and IV.
			using (var aes = Aes.Create())
			{
				aes.Key = key;
				aes.IV = iv;

				// Create a decrytor to perform the stream transform.
				ICryptoTransform decryptor = aes.CreateDecryptor(aes.Key, aes.IV);

				// Create the streams used for decryption.
				using (var msDecrypt = new MemoryStream(cipherText))
				using (var csDecrypt = new CryptoStream(msDecrypt, decryptor, CryptoStreamMode.Read))
				using (var srDecrypt = new StreamReader(csDecrypt))
					// Read the decrypted bytes from the decrypting stream
					// and place them in a string.
					plaintext = srDecrypt.ReadToEnd();
			}

			return plaintext;
		}
	}
```

### Test - Aes256Chiper

```csharp
[Test]
public void EveryCryptographicResultsIsDifferentEvenInCaseOfSamePassawordAndValue()
{
	const string yes = "YES";
	const string no = "NO ";
	Guid pwd1 = Guid.NewGuid(), pwd2 = Guid.NewGuid();
	string[] values = {yes, yes, no, no, yes};

	Aes256Chiper chiper = new Aes256Chiper();

	string[] encriptedValues = values.Select(v => chiper.EncriptBase64(pwd1, pwd2, v)).ToArray();
	string[] decriptedValues = encriptedValues.Select(v => chiper.DecriptBase64(pwd1, pwd2, v)).ToArray();

	Assert.IsEmpty(encriptedValues.Intersect(values));
	Assert.IsTrue(decriptedValues.SequenceEqual(values));
	Assert.That(encriptedValues[0], Is.Not.EqualTo(encriptedValues[1]));
	Assert.That(encriptedValues[0], Is.Not.EqualTo(encriptedValues[2]));
	Assert.That(encriptedValues[0], Is.Not.EqualTo(encriptedValues[4]));
	Assert.That(encriptedValues[2], Is.Not.EqualTo(encriptedValues[3]));

#if DEBUG
	for (int i = 0; i < values.Length; i++)
		Console.WriteLine($"{values[i]} => {encriptedValues[i]}");
#endif
}
```

### Test - Output

```
YES => EAAAAHCen3c2TZ+RnqdxBuKQFKokYIZjPefxYSwFWRTSq2IO
YES => EAAAAJ8pQjPbLOCJ1NF0f6GkGJmrINUfuf4Sm9pAoT/bmt+p
NO  => EAAAAPNiXuzR1wN2bm/xhtbussOr2qCgnxOJlOTOOajsUn63
NO  => EAAAAJk8QXfltrU1UCzd5XXhscd038YUcaT80ZQarg0qriLC
YES => EAAAAD3OkyyDm70ECORDNBecAh3EKCNFgJZPaiXjU4F2BaC/
```

> You can view my code and tests in my [FxCommonStandard Open Source Project](https://github.com/waldrix/FxCommonStandard)
