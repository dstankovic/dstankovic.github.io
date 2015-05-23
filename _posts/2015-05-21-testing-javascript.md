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

First thing you'll need to do is to download latest Jasmine library from [here](https://github.com/jasmine/jasmine/releases).

In this purposes I created simple 'program' which takes a string from an input and encrypt it with [Caesar cipher](http://en.wikipedia.org/wiki/Caesar_cipher).

So first, here is our index.html file:

{% highlight html %}
<html>
  <body>
    <input type="text" name="normalText">
    <button onclick="caesar()">Caesar cipher</button>
    <br>
    <script src="Caesar.js" type="text/javascript"></script>
  </body>
</html>
{% endhighlight %}

And here is our Caesar.js file:

{% highlight javascript %}

var caesar = function(){
  //get value from input
  var value = document.getElementsByName('normalText')[0].value;
  var encryptedText = "";
  for(var i=0;i<normal.length;i++){
    encryptedText += String.fromCharCode(normal.charCodeAt(i)+3);
  }
  alert(encryptedText);
}
{% endhighlight %}

What we need to understand is that our code is not always testable and you have to make an effort to make it to be. Another important thing is that we need to put a focus on testing our business logic. So if you want to test DOM manipulation you better take a look at end-to-end testing.
Having these things in mind we can notice that all of our code is into one function and it's much harder to test it. What we're going to do is to move text manipulation part into another function so it looks like this:

{% highlight javascript %}
var encrypt = function(normal){
  var encryptedText = "";
  for(var i=0;i<normal.length;i++){
    encryptedText += String.fromCharCode(normal.charCodeAt(i)+3);
  }
  return encryptedText;
}

var caesar = function(){
  //get value from input
  var value = document.getElementsByName('normalText')[0].value;
  var encrypted = encrypt(value);
  alert(encrypted);

}
{% endhighlight %}

So after we separated out 'business' logic into encrypt function we made it easier for testing. Now we are going to crate another file called CaesarTest.js and put it into test folder.

{% highlight javascript %}
describe('CaesarTest', function(){

  it('should transform all letters in given number of times to be three positions above in ascii order',
    function(){
      expect(encrypt('123')).toEqual('456'); //matcher
      expect(encrypt('abc')).toEqual('def');
      expect(encrypt('333')).toEqual('666');
  });
});
{% endhighlight %}

Now, lets add a check in encrypt function. If value entered is not a string or is undefined return string 'ERROR'. Add the following code at the beginning of encrypt function

{% highlight javascript %}
...
if(!normal || typeof normal != 'string'){
  return "ERROR";
}
...
{% endhighlight %}

We don't want to leave this code untested, so lets add another test. We just need to add new it function, describe it and provide test function.

{% highlight javascript %}
it('shoud return ERROR if text is not string or is undefined',function(){
  expect(encrypt(undefined)).toEqual('ERROR');
  expect(encrypt(4)).toEqual('ERROR');
});
{% endhighlight %}
You can also add checks for null or object but I'll leave that to you.

Ok, so now that we wrote our two tests we need to somehow run it. Standalone library comes with SpecRunner.html file and an example how to run tests. So we will just modify file and run it in browser to check the test results.

{% highlight html %}
<html>
<head>
  <link rel="shortcut icon" type="image/png" href="lib/jasmine-2.3.4/jasmine_favicon.png">
  <link rel="stylesheet" href="lib/jasmine-2.3.4/jasmine.css">

  <script src="lib/jasmine-2.3.4/jasmine.js"></script>
  <script src="lib/jasmine-2.3.4/jasmine-html.js"></script>
  <script src="lib/jasmine-2.3.4/boot.js"></script>

  <!-- include source files here... -->
  <script src="Caesar.js"></script>

  <!-- include test files here... -->
  <script src="tests/CaesarTest.js"></script>
</head>

<body>
</body>
</html>
{% endhighlight %}

We shound end up with this in our browser
![](/images/test-result.png)
