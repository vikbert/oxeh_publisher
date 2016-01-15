title: Write acceptance tests with codeception
tags: BDD, codeception
categories: Tests
description: This is a brief step by step guide to use `CodeCeption` to create a basic but very useful tests for API endpoint. I do prefer Codeception over Behat for web acceptance, specially if some tests should be implemented as simple as possible and as faster as possible.
toc: true
---

This is a brief step by step guide to use CodeCeption to create a basic but very useful tests for API endpoint.

### Set up environment

apply the following commands in a terminal to set up codeception test environment:
```bash
mkdir codeception-tests
cd codeceptions-tests
composer search codeception
composer require codeception/codeception
vendor/bin/codecept bootstrap
vendor/bin/codecept run
```

### First single Test

> Test scenario: I want to test if the localhost on the mac works well. As expected, if open http://localhost in a browser, the message “it works” should be displayed in the browser.

#### creat a test file
```php
<?php
// .tests/acceptance/LocalhostCept.php
$tester = new AcceptanceTester($scenario);
$tester->wantTo('ensure that localhost works well');
$tester->amOnPage('/');
$tester->see('it works');
?>
```

#### config acceptance.suite.yml
```
class_name: AcceptanceTester
modules:
    enabled:
        - PhpBrowser:
            url: http://localhost
        - \Helper\Acceptance
```

#### run demo test
```bash
php vendor/bin/codecept run
```

It should show that the first acceptance test is executed and works well.

```
Codeception PHP Testing Framework v2.1.4
Powered by PHPUnit 4.8.18 by Sebastian Bergmann and contributors.

Acceptance Tests (1) --------------------------------------------
Ensure that localhost on the current machine works well Ok

Time: 145 ms, Memory: 11.75Mb
generate test suite for REST APIs
```

### Write test suite

#### generate test suite
I tried to use an online REST API api.github.com as test target. For the tests, a test suite can be created via:
```bash
vendor/bin/codecept generate:suite github
```

#### config test suite.yml
This will generate the xml configuration file named github.suite.yml, then update the config with following content:

```
class_name: GithubTester
modules:
    enabled:
        - REST
            url: https://api.github.com
            depends: PhpBrowser
            part: Json
```


#### build helper classes
then we generate the test helper classes for the test suite

```bash
vendor/bin/codecept build
```

#### create test to the suite
once we have configured the new test suite, create the first test:

```bash
vendor/bin/codecept generate:cept github Users
```

> It will be called UsersCept.php. We can use it to test the all RESTful endpints via the REST api.

#### implement the test
Now open the UsersCept.php and add the test like the following:
```php
<?php
    $tester = new githubTester($scenario);
    $tester->wantTo('get the list of github users');
    $tester->sendGET('/users');
    $tester->seeResponseCodeIs(200);
    $tester->seeResponseIsJson();

    $tester = new githubTester($scenario);
    $tester->wantTo('get the single user details');
    $tester->sendGET('/user');
    $tester->seeResponseCodeIs(401);
    $tester->seeResponseContains('Requires authentication');
?>
```

#### run the test suite

```bash
vendor/bin/codecept run github
```

### Conclusion

I do prefer Codeception over Behat for web acceptance, specially if some tests should be implemented as simple as possible and as faster as possible.
As we have showed in the demo, codeception bundled with modules for functional and acceptance tests: PhpBrowser (functional test over curl), selenium for acceptance tests etc. There are also some very nice modules for REST that can help the developers to test the REST API.
