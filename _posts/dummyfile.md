---
published: false
---

# Testing JavaScript
## Protect yourself from yourself. Write unit tests.

1. Why you should write tests?
2. TDD and BDD
3. Frameworks
4. Examples in Jasmine (plain JS)
5. How we use it  Jasmine/Karma/Grunt (AngularJS)


### Why you should write tests?

Every programer had that awkward moment when something went terribly wrong after a new release. Then you have to run through code, find a bug fix it, patch it, etc... The worst part is that thing previously was working nice. So what probably happened is that when someone changed a thing, it messed up another thing. It’s not so rare to happen. Speaking of this we come to first reason why you should write unit tests, to prevent **regressions**. Second reason is that it may influence readability and organisation of your code. For example, you maybe wrote some code which is not so easy to test. Then you have to **refactor** it a bit, maybe divide it into **smaller chunks** and then test each of them.  

### TDD and BDD

The most common approach to testing is TDD. In TDD, you test “unit of implementation” in this specific order:  1. write falling test; 2. implement functionality and make the test pass; 3. refactor code; BDD is derived from TDD and process is the same, but instead of writing test for functionality, you write a test for specific behaviour. 
