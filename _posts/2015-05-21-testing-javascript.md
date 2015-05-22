---
layout: post
title: Testing JavaScript
---

# Testing JavaScript

## Protect yourself from yourself. Write unit tests.

1. Why you should write tests?
2. TDD and BDD
3. Frameworks
4. Getting started with Jasmine
5. How we use it  Jasmine/Karma/Grunt (AngularJS)


### Why you should write tests?

Every programer had that awkward moment when something went terribly wrong after a new release. Then you have to run through code, find a bug, fix it, patch it, etc... The worst part is that thing previously was working nice. So what probably happened is that when someone changed a thing, it messed up another thing. It’s not so rare to happen. Speaking of this we come to first reason why you should write unit tests, to prevent **regressions**. Second reason is that it may influence readability and organisation of your code. For example, you maybe wrote some code which is not so easy to test. Then you have to **refactor** it a bit, maybe divide it into **smaller chunks** and then test each of them.  

### TDD and BDD

The most common approach to testing is TDD. In TDD, you test “unit of implementation” in this specific order:  1. write falling test; 2. implement functionality and make the test pass; 3. refactor code; BDD is derived from TDD and process is the same, but instead of writing test for functionality, you write a test for specific behaviour.

### Frameworks

There is literally a bunch of [JS testing frameworks](http://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#JavaScript "Js Testing frameworks") today, but I’ll go through most common one, QUnit, Mocha and Jasmine.


![JavaScript Frameworks](/images/jsframework.png)


###[QUnit](https://qunitjs.com)

QUnit is the oldest of these three.You need just div in your html and you are ready to go. It has simple API.
It has stop()/start() functions to handle asynchronous code testing.

{% highlight javascript %}
//example
QUnit.test('simple test increment',function(){
    var testingObject = {id: 5, count: 10};

    //asume this function returns n+1 and we want to test it
    testingObject.count = count(testingObject.count);
    assert.equal(testingObject.count, 11, 'OK');


    testingObject.count = count(testingObject.count);
    assert.equal(testingObject.count, 12, 'OK');

});
{% endhighlight %}

###[Mocha](http://mochajs.org/)

Mocha is another popular testing framework. It doesn't have assertion library but you can choose one of many.
It has a bunch of features and its built on top of [Node.js](https://nodejs.org).

{% highlight javascript %}
var assert = require("assert"); // node.js core module

  describe('increment function', function(){
    it('should return +1 count when function is called', function(){
      var testingObject = {id: 5, count: 10};

      //function takes whole object and change its count by 1
      count(testingObject);
      assert.equal(testingObject.count, 11);
    })
  });

{% endhighlight %}

###[Jasmine](http://jasmine.github.io/)

Jasmine is, according to [GitHub](http://github.com) stars, most popular testing framework. It has great asertion library and also (as Mocha) great tools to test asynchronous code. Code style is same as above for Mocha (BDD), but it has it's own assert library.
From here on I'll give a basic example how to start with Jasmine and how we use it with AngularJS/Grunt/Karma.

### Getting started with Jasmine
