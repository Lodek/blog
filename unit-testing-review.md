# A review for Unit Testing Principles, Practices and Patterns @2021-08-15
TLDR: Review and summary of the book "Unit Testing Principles, Practices and Patterns" by Vladimir Khorikov.
The intent is to summarize what I considered some important take aways from the book.

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
3. Why test
4. Unit test principles
5. Integration tests practices
6. Patterns

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

## Why unit test

The author starts off by justifying why applications should have unit tests.
The key takeaway presented is: susteinable growth.

Not too long ago companies were resistent to software testing.
It was seen as something that lengthened the development process and was bad for business.
As most tech companies have learnt, there's more to it than that.

Although testing may delay the initial delivery of results, as software grows, its main developers have moved on to different projects and jobs, the code base is no longer fully understood by the team and that leads to problems.

Testing provides a safety net for developers to refactor and add features with a minimum guarantee that they have not broken anything.
No code base is perfect and some coupling will exist, changes will cause unexpected side effects in the production code and that's how regressions are born.

A good set of tests delivered alongside the main feature enables a software project to grow by minimizing the chaos.

The author arguees that as code is added and different developers work on the code base, the code base entropy increases.
Increased entropy leads to a more complicated code base which increases the odds of regressions.
Without tests, the chaotic nature of software growth will spiral out of control.
Thus tests act as a safety net for developers to change things, hopefully improve the legated code base while keeping the current set of features working.
By enabling developers to work and modify the code base, tests provide the sustainable growth of the software project, hence being of value to the developers because it acts as an indicator that nothing is broken and to the company because it minimizes bugs found by clients, which can damage their reputation.

## Unit testing principles

The purpose of unit tests is to enable the sustainable growth of a project but not every test suite does that.
A unit test is only valuable if it contributes to this goal.
Some tests are nothing more than a maintanence burden, with no real value in the code base while others are critical in catching regressions.
How to write tests that contribute, rather than hinder, the goal of sustainability?
That's what 3/4 of the book talks about, it'd be impossible to give a comprehensive answer to that question.
With that said, the author introduces some concepts which can be used as a North to structuring and classifying tests.

### Unit testing characteristics
To write a unit test first it's important to understand what makes up a unit test.
This definition is a common source of misunderstandings and arguments.
TLDR: Unit tests are automated tests; they verifiy a small piece of code; they run quickly; they run in an isolated manner.

There are two schools of thought to unit tests, as the author explain, the "London approach" and the "Classical approach".

In the London approach "running in an isolated manner" means that a Unit test tests a class and only that class.
Any and all dependencies of the testee must be mocked out.
Isolation is done at a class level.

The Classical approach understand that "isolation" refers to tests, not classes.
A *test* should be run in an isolated manner, which is to say, a test should have no side effect which allows for parallel execution.
What that entails is that only out-of-process dependencies are mocked out.
If the application being tested modifies an external service, during a test this interaction should be mocked as to not produce side effects, which may introduce flaky tests or race conditions.
In the case of an application database, one may use transactions / atomic operations around each test case, the author goes in depth about the subject in later parts of the book.

The book favors the classical approach and gives arguments as to why it's a best take in the long run.
The main points are that unit tests should test a unit of application behavior not code.
The London approach makes it impossible to do so because non-trivial systems requires multiple classes to implement a business behavior, by mocking out the classes involved in the execution path of this behavior, the test isn't truly testing the behavior, only the interactions with the collaborators.

Again, this is a complex topic and although I strongly agree with the author in that the Classical approach does produce better tests and it is less burdersome, each scenario is different.
If may very well be that the codebase is a complete mess of spaghetti code that a method's scope is not even well defined.
Another scenario would be a poorly architectured application where unexpected classes call out of process dependencies, violating the "isolation" attribute.

There isn't a one-size-fits all here, it's the job of the developers and engineers to evaluate each scenario and see what makes sense.
My one recommendation (and the book's) is to favor the classical approach when possible.
There are significant benefits to be gained, all of which the author elegantly presents in the book.


### The AAA pattern

Most unit tests follow a structure with 3 sections: Arrange, Act and Assert.

In the Arrange section the unit test sets up mock, test data, value object or whatever is needed for the Act phase.
In the Act phase, the unit test uses the data setup in the Arrange phase and performs and action, usually by calling a method on the System under test (SUT).
The SUT usually produces and output or changes some state, it modifies the system somehow, in the Arrange section the unit test verifies that the SUT's output produced the desired effect (be it a state transition or a method output).

The AAA pattern defines a few guidelines of dos and don'ts.
The author explores the pattern in depth but I believe the main take away here is this: there should only be one action in the Act section.
That's important because according to the definition of unit tests, each test verifies an unit of behavior.
As general guideline, a public method in the SUT should be produce an output that matches a behavior, if multiple calls are required in the Act section it is a strong indication that the SUT has a leaky interface, which is to say, it is exposing implementation details to the calling code.
Having said that, remember that there is no silver bullet in engineering, evaluate each situation accordingly.

### Pillars of good tests

Having established the whys and the whats we get to the how of unit tests.
How to write tests that add value.

The author introduces 4 pillars to unit tests:

> 1 - Protection against regression<br>
  2 - Resistance to refactoring<br>
  3 - Fast feedback<br>
  4 - Maintainabilty

These four pillars lay the foundation through which one can think about tests, and indeed, that's how the author approaches the matter.
Throughout the book, the author revisits these pillars again and again to justify why an approach is more favorable than the other.
An example of that is the resistance to refactoring trait.
One of the reasons the London approach is seen as problematic is because it is weak to refactoring.

Protection against regression measures how well a test is able to catch a regression, that is, a bug that is created after a code change.
A test is valuable if it is able to identify bugs.
Identifying bugs early on is the very reason why tests enable a sustainable growth of the project.
Hence, it follows that if a test does not do a good job at catching regressions, it's not doing its job.

Resistance to refactoring relates to whether the test code requires refactoring after the production code is refactored.
The test suite is an extremely important part of the code base.
With that said, test code *is* code, which mean it requires human hours to maintain.
If the testing code breaks and requires fixing up after each code refactoring, the test may become a burden to the development team.
Ideally tests can withstand a minimum ammount of code refactoring without requiring intervention.
Care is required so the test suite doesn't become a technical burden which hinders the desired objective of susteinable growth.

Fast feedback relates to how quickly and often the tests can be run.
A quick feedback loop is essential to guarantee that development can be carried out effectively.
Fast running tests mean that local development can be carried out and its correctness can be continously verified by checking whether the tests are passing.

Maintainability relates to the costs of maintaining the test suite.
Reiterating, the test code is still code and depending the size of the project, it may be a significant part of the code base.
If the tests are long, complex or hard to understand, the cost of maintanence goes up.
Likewise, if the tests aren't unit tests at all and communicate with external dependencies, that introduces additional burdens related to configuring the environment and ensuring its consistency.
In general, as tests more closely simulate the end user's behavior (integration, end to end) the better it is at catching regression but the harder it is to maintain.

One can start using these four pillars to see where a test fits and whether it is worth maintaining.
A valuable takeway from these concepts is to note that not all tests are equal.
Some tests are valuable and others aren't.
Writing tests to cover every single function or method in the production code is unsustainable and wasteful.
Careful consideration should be put on which behaviors will be tested, which won't and whether the tests will protect the application from future changes.

What should be tested is a tricky subject with lots of particularities.
The book explores various axes regarding this issue.
A good guideline is to test behaviors and not any behavior, *observable* behaviors.
An observable behavior can be thought of an action performed by the system that changes the external world.
Examples are: data returned to the user, an email sent, a message passed onto a bus and so on.
With that said, testing infrastructure code isn't always valuable, the main focus should go on domain logic / algorithms and controllers.

In short, domain logic relates to code that encapsulate business logic (the author often mentions concepts related to domain driven design but even if an application doesn't apply DDD the idea can still be used).
Algorithms may or may not be related to the domain model, some algorithms are complex (and important) in their own right to require thorough testing.
Controllers can be thought of any code that orchestrate calls.
As a guideline the book recommends separating the code intro controllers and algorithms / domain model, which eases testing and creates a clearer boundary between the application and the business logic.

From a testing perspective, this practice enables domains and algorithms to follow a (mostly) black-box approach to testing algorithms and domain models, in which the code generates a values / domain events as outputs, which are easy to test.
Controllers are then test the entire application call stack, asserting that the code is invoking the correct collaborators.
Note that in this model controllers, and ideally only them, will make calls to external dependencies (apis, message bus, etc), which means that controller tests will use some sort of test double to remove these dependencies.
