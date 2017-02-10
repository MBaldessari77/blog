---
layout: post
title: "Continuos Integration & Static code analisys"
date: 2017-02-10
---

Relatively recent, my team started to look and introduce [Agile practices](https://en.wikipedia.org/wiki/Category:Agile_software_development), with the objective to improve software quality and stability.

We introduce [Continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) as first pratctice, to Continuos build and test code commited (with git in the development branch in our case).

Looking ahead in most known open source projects my intresting falling into this badge <img href="https://bestpractices.coreinfrastructure.org/projects/29/badge"></img> on [Node.js](https://github.com/nodejs/node) open source project site.

I found intresting the [CII Best Practices Badge Program](https://bestpractices.coreinfrastructure.org/) then I suggest and introduced [Static code analisys](https://github.com/linuxfoundation/cii-best-practices-badge/blob/master/doc/criteria.md#analysis) criteria in our web projects and integrated in the C.I. process.

## Practically

1. Choose the tool

We chose [JSHint](http://jshint.com/about/) as Static Code Analysis Tool (see also [List of tools for static code analysis](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis))

2. Install jshint with npm

```
npm install -g jshint
```

3. Configure task in Gruntfile.js on the Web Site folder project

```javascript
    ...
    // jshint
    jshint: {
      all: ['app/**/*.js'], //Analize all file under app folder
      options: {
        reporter: require('jshint-stylish'),
        strict: true, //Force strict (see http://jshint.com/docs/options/#strict)
        asi: true, //Disable ; check (see http://jshint.com/docs/options/#asi)
        eqeqeq: true, //Enforce === o !== use (see http://jshint.com/docs/options/#eqeqeq)
        eqnull: true, //Disable == null control (see http://jshint.com/docs/options/#eqnull)
        browser: true, //Defines globals exposed by modern browsers  (vedi http://jshint.com/docs/options/#browser)
        globals: {
          jQuery: true //Include jQuery definitions (vedi http://jshint.com/docs/options/#jquery)
        }
      }
    }
    ...
```

4. Include Static code analisys step in new analisys stage on C.I. server for the project (call inline with bash/cmd)

```
grunt jshint
```

## Conclusion

With a simple practice, and with an initial minimal friction, whe increase the stability of our client code and reduce impact with [minification process](https://en.wikipedia.org/wiki/Minification_(programming)). This allowed us to **save much time later** to find errors wich crash the minification step!