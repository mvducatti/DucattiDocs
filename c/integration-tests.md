# Integration Tests

## Criando Testes de Integração

{% embed url="https://www.youtube.com/watch?v=7roqteWLw4s&t=330s" %}

{% embed url="https://code-maze.com/integration-testing-asp-net-core-mvc/" %}

## \[Integration vs Unit\] Testing

{% embed url="https://stackoverflow.com/questions/10752/what-is-the-difference-between-integration-and-unit-tests" %}

The key difference, to me, is that **integration tests** reveal if a feature is working or is broken, since they stress the code in a scenario close to reality. They invoke one or more software methods or features and test if they act as expected.

On the opposite, a **Unit test** testing a single method relies on the \(often wrong\) assumption that the rest of the software is correctly working, because it explicitly mocks every dependency.

Hence, when a unit test for a method implementing some feature is green, it does **not** mean the feature is working.

Say you have a method somewhere like this:

```text
public SomeResults DoSomething(someInput) {
  var someResult = [Do your job with someInput];
  Log.TrackTheFactYouDidYourJob();
  return someResults;
}
```

`DoSomething` is very important to your customer: it's a feature, the only thing that matters. That's why you usually write a Cucumber specification asserting it: you wish to verify and communicate the feature is working or not.

```text
Feature: To be able to do something
  In order to do something
  As someone
  I want the system to do this thing

Scenario: A sample one
  Given this situation
  When I do something
  Then what I get is what I was expecting for
```

No doubt: if the test passes, you can assert you are delivering a working feature. This is what you can call **Business Value**.

If you want to write a unit test for `DoSomething` you should pretend \(using some mocks\) that the rest of the classes and methods are working \(that is: that, all dependencies the method is using are correctly working\) and assert your method is working.

In practice, you do something like:

```text
public SomeResults DoSomething(someInput) {
  var someResult = [Do your job with someInput];
  FakeAlwaysWorkingLog.TrackTheFactYouDidYourJob(); // Using a mock Log
  return someResults;
}
```

You can do this with Dependency Injection, or some Factory Method or any Mock Framework or just extending the class under test.

Suppose there's a bug in `Log.DoSomething()`. Fortunately, the Gherkin spec will find it and your end-to-end tests will fail.

The feature won't work, because `Log` is broken, not because `[Do your job with someInput]` is not doing its job. And, by the way, `[Do your job with someInput]` is the sole responsibility for that method.

Also, suppose `Log` is used in 100 other features, in 100 other methods of 100 other classes.

Yep, 100 features will fail. But, fortunately, 100 end-to-end tests are failing as well and revealing the problem. And, yes: **they are telling the truth**.

It's very useful information: I know I have a broken product. It's also very confusing information: it tells me nothing about where the problem is. It communicates me the symptom, not the root cause.

Yet, `DoSomething`'s unit test is green, because it's using a fake `Log`, built to never break. And, yes: **it's clearly lying**. It's communicating a broken feature is working. How can it be useful?

\(If `DoSomething()`'s unit test fails, be sure: `[Do your job with someInput]` has some bugs.\)

Suppose this is a system with a broken class: ![A system with a broken class](https://i.stack.imgur.com/611ZY.jpg)

A single bug will break several features, and several integration tests will fail.

![A single bug will break several features, and several integration tests will fail](https://i.stack.imgur.com/rKhSr.jpg)

On the other hand, the same bug will break just one unit test.

![The same bug will break just one unit test](https://i.stack.imgur.com/M64Vb.jpg)

Now, compare the two scenarios.

The same bug will break just one unit test.

* All your features using the broken `Log` are red
* All your unit tests are green, only the unit test for `Log` is red

Actually, unit tests for all modules using a broken feature are green because, by using mocks, they removed dependencies. In other words, they run in an ideal, completely fictional world. And this is the only way to isolate bugs and seek them. Unit testing means mocking. If you aren't mocking, you aren't unit testing.

### The difference

Integration tests tell **what**'s not working. But they are of no use in **guessing where** the problem could be.

Unit tests are the sole tests that tell you **where** exactly the bug is. To draw this information, they must run the method in a mocked environment, where all other dependencies are supposed to correctly work.

That's why I think that your sentence "Or is it just a unit test that spans 2 classes" is somehow displaced. A unit test should never span 2 classes.

