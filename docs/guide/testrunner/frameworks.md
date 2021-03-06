name: frameworks
category: testrunner
tags: guide
index: 2
title: WebdriverIO - Test Runner Frameworks
---

Frameworks
==========

The wdio runner currently supports [Mocha](http://mochajs.org/), [Jasmine](http://jasmine.github.io/) (v2.0) and
[Cucumber](https://cucumber.io/). These BDD frameworks are all based on Node.js and need to be installed via
NPM. Note that the modules need to be installed at the same place WebdriverIO is installed. If you installed
WebdriverIO globally make sure you have the test frameworks installed globally as well.

## Using Mocha

If you like to use Mocha you should additionally install an assertion library that supports promises,
e.g. [Chai As Promise](http://chaijs.com/plugins/chai-as-promised). Initialise that library in the
`before` hook in your configuration file:

```js
before: function() {
    var chai = require('chai');
    var chaiAsPromised = require('chai-as-promised');
&nbsp;
    chai.use(chaiAsPromised);
    expect = chai.expect;
    chai.Should();
}
```

Once that is done you can write beautiful assertions like:

```js
it('should do some chai assertions', function() {
    return browser
        .url('http://webdriver.io')
        .getTitle().should.eventually.be.equal('WebdriverIO');
});
```

Make sure to checkout the [transfer promises](/guide/usage/transferpromises.html) section to enable multiple
consecutive assertions. If you want to define specific Mocha settings you can do that by adding `mochaOpts`
to your configuration file.

There are three different approaches how to deal with asynchronicity in your `it` blocks:

###### The Good Old Callback Way

```js
describe('my feature', function() {
    it('will do something', function(done) {
        browser
             .url('http://google.com')
             .getTitle().then(function(title) {
                 console.log(title);
              })
              .call(done);
    });
});
```

###### Using Promises

```js
describe('my feature', function() {
    it('will do something', function() {
        return browser
             .url('http://google.com')
             .getTitle().then(function(title) {
                 console.log(title);
              });
    });
});
```

###### The New Fancy Generator way

Note that you shouldn't forget the asterisks after `function`.

```js
describe('my feature', function() {
    it('will do something', function* () {
        yield browser.url('http://google.com');
        var title = yield browser.getTitle()
        console.log(title);
    });
});
```

## Using Jasmine

Jasmine already provides assertion methods you can use with WebdriverIO. Also the way how you deal with
asynchronicity is the same like using Mocha. The only difference is that you can change Jasmines
`defaultTimeoutInterval` by settings it in the `jasmineNodeOpts` in your configuration file.

## Using Cucumber

If you want to use Cucumber set the `framework` property to cucumber, either by adding `framework: 'cucumber'`
to the [config file](/guide/testrunner/configurationfile.html) or by adding `-f cucumber` to the command line.

Options for Cucumber such as 'format' can be given in the config file with cucumberOpts:

```js
cucumberOpts: {
  format: "summary"
}
```