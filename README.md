# radialbars-test
radialbars UX Feature Testing Automation

[![Build Status](https://travis-ci.org/christrees/radialbars-test.svg?branch=master)](https://travis-ci.org/christrees/radialbars-test)

***

radialbars-test project to run WebdriverIO tests with [Cucumber](https://cucumber.io/) and follow [TTD](https://en.wikipedia.org/wiki/Test-driven_development) and [BDD](http://en.wikipedia.org/wiki/Behavior-driven_development) to create a [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) for [radialbars](https://github.com/christrees/radialbars)

## Requirements

- Node version 4.8 or higher

Although this project works fine with NPM we recommend to use Yarn (>= 1.0.0) instead,  due to its speed & solid dependency locking mechanism. To keep things simple we use yarn in this guide, but feel free to replace this with NPM if that is what you are using.

## Quick start

1. Clone the git repo — `git clone https://github.com/christrees/radialbars-test.git`

2. Install the dependencies 

```sh
$ yarn install
```

4. Run the tests

```sh
$ yarn run test
```


# How to write a test

Tests are written in [Gherkin syntax](https://cucumber.io/docs/reference)
that means that you write down what's supposed to happen in a real language. All test files are located in
`./src/features/*` and have the file ending `.feature`. You will already find some test files in that
directory. They should demonstrate, how tests could look like. Just create a new file and write your first
test.

__gamePlay.feature__
```gherkin
Feature: radialbars game play
    As a Developer in Test
    I want to play radialbars
    So that I can verify game play in my feature tests

Scenario: open game app url
    Given I open the url "https://christrees.com/radialbars/"
    Then  I expect that the url is "https://christrees.com/radialbars/"
    And   I expect that the title is "rb-test"
#    When  I pause for 10000ms

Scenario: Start Game
    Given I open the url "https://christrees.com/radialbars/"
    Then  I expect that the url is "https://christrees.com/radialbars/"
    And   there is an element ".hub" on the page
    And   the element "text" matches the text "Click to Start"
    When  I click on the element ".hub"
    Then  I expect that element ".bar" does appear exactly "1" times
    And   the element "text" matches the text "0"
    When  I pause for 5000ms
    Then  the element "text" matches the text "Game Over"

#@WIP @Pending
Scenario: Click bar
    Given I open the url "https://christrees.com/radialbars/"
    And   there is an element ".hub" on the page
    And   the element "text" matches the text "Click to Start"
    When  I click on the element ".hub"
    Then  I expect that element ".bar" does appear exactly "1" times
    And   I expect that element ".bar" becomes visible
    And   I pause for 2000ms
    When  I move and click the radialbars element ".bar"
    And   I pause for 100ms
    Then  the element "text" matches the text "+ 5"
    And   I pause for 1000ms
    Then  the element "text" matches the text "5"
    When  I pause for 5000ms
    Then  the element "text" matches the text "Game Over"

```

This test opens the browser and navigates them to https://christrees.com/radialbars/ to check if the title is rb-test. As you can see, it is written as pretty simple text.  This is Gherkin.  The purpose of using Gherkin to the creation, validation and maintainance of a Domain Specific Language (DSL).  As development proceeds, use of the DSL will capture, validate and maintain a consistant discription of the language used.  This process helps the various stakeholders (users, developers, writers, artist) maintain the User Experience (UX).

# Configurations

To configure your tests, checkout the [`wdio.conf.js`](https://github.com/webdriverio/cucumber-boilerplate/blob/master/wdio.conf.js) file in your test directory. It comes with a bunch of documented options you can choose from.

## Environment-specific configurations

You can setup multiple configs for specific environments. Let's say you want to have a different `baseUrl` for
your local and pre-deploy tests. Use the `wdio.conf.js` to set all general configs (like mochaOpts) that don't change.
They act as default values. For each different environment you can create a new config with the following name
scheme:

```txt
wdio.<ENVIRONMENT>.conf.js
```

Now you can create a specific config for your pre-deploy tests:

__wdio.PageObjectTest.conf.js__
```js
const wdioConfig = require('./wdio.conf.js');

wdioConfig.config.capabilities = [{
    browserName: 'chrome',
}];
wdioConfig.config.logLevel = 'silent',
wdioConfig.config.baseUrl = 'https://christrees.com/radialbars/',
wdioConfig.config.specs = [ __dirname + '/src/pospecs/*.spec.js' ],
wdioConfig.config.services = ['selenium-standalone', 'visual-regression'];
wdioConfig.config.framework = 'mocha';

exports.config = wdioConfig.config;
```

Your environment-specific config file will get merged into the default config file and overwrites the values you set.
To run a test in a specific environment just add the desired configuration file as the first parameter:

```sh
$ yarn run wdio wdio.PageObjectTest.conf.js
```

# Running single feature
Sometimes its useful to only execute a single feature file, to do so use the following command:

```sh
$ yarn run wdio -- --spec ./test/features/githubSearch.feature
```


# Using tags

If you want to run only specific tests you can mark your features with tags. These tags will be placed before each feature like so:

```gherkin
@Tag
Feature: ...
```

To run only the tests with specific tag(s) use the `--cucumberOpts.tagExpression=` parameter like so:

```sh
$ yarn run wdio -- --cucumberOpts.tagExpression=@Tag,@AnotherTag
```

You can add multiple tags separated by a comma

# Resources of note

- [This repo radialbars-test](https://github.com/christrees/radialbars-test)
- [Test repo radialbars](https://github.com/christrees/radialbars)
- [Deployed radialbars - github hosted](https://christrees.com/radialbars/)
- [Travis CI](https://travis-ci.org/)
- [Travis CI with Lambda Test](https://www.lambdatest.com/support/docs/travis-ci-with-lambdatest/)
- [Selenium tips on Lambda Test](https://www.lambdatest.com/blog/tips-for-test-automation-with-selenium/)

