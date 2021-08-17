# A review for Unit Testing Principles, Practices and Patterns @2021-08-15
Reference: 
```yaml
title: Unit Testing Principles, Practices, and Patterns
authors:
  - Vladimir Khorikov
publisher: Manning Publications
year: 2020
isbn: "9781617296277"
```
[link](https://www.google.com.br/books/edition/Unit_Testing_Principles_Practices_and_Pa/CbvZyAEACAAJ)

## Outline

1. Book abstract
2. Why I read this book and my background with testing (optional)
3. TBD

Book metadata:

```yaml
title: Unit Testing Principles, Practices, and Patterns
authors:
  - Vladimir Khorikov
publisher: Manning Publications
year: 2020
isbn: "9781617296277"
```

## Book abstract
The book "Unit Testing Principles, Practices and Patterns" takes a different approach to explaining tests and unit tests.

Right off the bat, the author justifies the importance of tests and their purpose.
With their importance established, the author starts exploring the subject of unit tests from the prespective of a set of attributes that exists in "good" tests.
These attributes are: resistance to refactoring, protection against regression, maintanability and execution speed.

The attributes acts as guiding principles which guide the dicussion about unit testing.
From these discussions the author explores different testing topics and introduces concepts to help in writing effective tests.

Althouh this book is focused on tests, Vladimir writes about applications accounting for common architectural practices.
Several examples in the book assume enterprise applications designed using MVC and DDD.
The beauty of this approach is that the book teaches about code architecture while teaching tests.
It might be no surprise as "good" code is often easier to test but nevertheless, the dicussions about application architecture are excellent and very eye opening.
The author touches on several axis of enterprise applications such as: databases, multi layer architecture, message passing and functional architecture.
The end result is that the discussions are rich, full of context and tons of incredible information.

## Why I read this book and my background with testing
NOTE: This section is optional, skip it to save time.

My initial contact with Unit Testing was sometime in 2018, in the context of Python, through an excellent [talk](https://nedbatchelder.com/text/test0.html) by Ned Batcher.
Ned's talk is an excellent resource to get started and to start writing tests to save yourself time in the future.
At the time I watched his talk I was not working with any big software projects, all I had where my own toy projects.
Ned's talk got me started with tests and testing helped me save my own time while coding.

Although the idea behind unit tests is simple, executing them properly is hard and there's only so much a talk, no matter how well elaborate it is, can teach.
Often times I'd feel some friction while writing tests.
I had mixed feelings about it because some of it seemed really valuable and some of it not so much.
Practices like TDD seemed alien to me because at the time, hardly any project I worked in had a well drawn architecture, I was figuring things out as I went.

The problem aggravated when I was introduced to Unit Testing at a professional level in Java.
The company I worked for had strict code testing policies, aiming for 100% coverage, and made use of extensive mocking, something that baffled me.
Tests were written such that any class would have all of its dependencies mocked out.
No class would interact with any concrete class in the context of testing.

This approach, which can be called the "London approach" to testing bothered me.
A lot.
I struggled to understand how those tests were sustainable, writting them was an actual pain and they seemed so brittle.
Naturally changing the interface of class requires refactoring, but with this approach, the work would be doubled.
Not only would I have to refactor the calling code, I would have to *also* refactor every mocked instance of the callee.

Another thing that bothered me was how tightly couple the test was to its internal implementation.
It's often said unit tests are black-boxes, feed some input, get the output, assert over the output.
It sure didn't feel like that when I had to configure mock behaviors for every other line of production code.
Enough berating, let's just say I was puzzled by the practice and I never came to terms with it.

I was left in a situation where I understood the need for tests but I couldn't make sense of some practices that were used out there.
Furthermore, I struggled to find a way to write tests that actually felt valuable.
I acknowledge this gap in my knowledge and eventually I had enough and decided to study about it.

Vladimir's book stood out as being well reviewed and after reading the introduction the book would shed some light on the issues.
The author mentioned how he felt a similar struggle to mine and some distaste for the London approach.
Maybe I was motivated by some some form of confirmation bias but I decided to read the book and it was a great decision.

The book is thorough in its analysis and it gives pros and cons of each scenario.
It is opinioanated in that it favors one approach, however it gives solid reasoning as to doing so.

More than anything else, this books has taught me to *think* about tests.
The reality is that tests are important but they also have a cost.
Writing tests for a project is about finding the balance of maximizing the chance of catching regressions and minimizing the ammount of maintanence the tests will need.
Finding this sweet spot and deciding on what should and what shouldn't be tested explicitly is the key.
