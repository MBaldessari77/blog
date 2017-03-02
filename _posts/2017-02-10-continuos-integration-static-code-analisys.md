---
title: "Continuos Integration & Static code analisys"
layout: post
date: 2017-02-10
categories: best practices
---

Relatively recent my team started to look and introduce [Agile practices](https://en.wikipedia.org/wiki/Category:Agile_software_development) with the objective to improve software quality and stability.

We choose [Continuous integration](https://en.wikipedia.org/wiki/Continuous_integration)n as first practice, to continuous build and test code committed.

Looking ahead in most known open source projects my interesting falling into this badge <a href="https://bestpractices.coreinfrastructure.org/"> <img src="https://bestpractices.coreinfrastructure.org/projects/1/badge"></a> on [Node.js](https://github.com/nodejs/node) project site.

I found interesting the [CII Best Practices Badge Program](https://bestpractices.coreinfrastructure.org/) then I suggest and introduced [Static code analisys](https://github.com/linuxfoundation/cii-best-practices-badge/blob/master/doc/criteria.md#analysis) criteria in our web projects and integrated in the C.I. process.

## Practically

*   Choose the tool

We chose [JSHint](http://jshint.com/about/) as Static Code Analysis Tool (see also [List of tools for static code analysis](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis))
  
*   Install jshint with npm

Follow [Download and install](http://jshint.com/install/) instructions

*   Personalize jshint task (optional)

Edit Gruntfile.js located in the Web Site folder project

```javascript
{
...
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
...
}
```

*   Include Static code analisys step in C.I.

```
grunt jshint
```

## Conclusion

With a simple practice, and with an initial minimal friction, we increase the stability of our client code and reduce impact with release build and deploy process. This allowed us to save much time later, for example, searching reasons for crash in the [minification step](https://en.wikipedia.org/wiki/Minification_(programming))!
