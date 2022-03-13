---
templateKey: blog-post
title: Individually wrapped assorted assertions
date: 2020-08-15T01:05:22.412Z
description: >-
  Good tests stand by an application from beginning to end. Choosing the wrong
  assertion for a test can make a test more expensive, fragile, or devalue it
  entirely. There are many different approaches to your assertion and there is
  no one assertion to rule them all. 
featuredpost: true
featuredimage: /img/lolly.jpg
tags:
  - Assertion
  - Testing
  - Engineering
  - Quality
  - Agility
  - Robustness
---
![Unwrapped lolly with wrapped lollies in the background](/img/lolly.jpg "Unwrapped lolly")

When it comes to our tests, if our tests are green we're satisfied and more or less ready to roll this change out to production. When our tests fail we desperately need information, but what do we do if your tests give us almost no hint at all.  

```
expected <true> but was <false>
```

Typically this sort of feedback may come from an assertion that we commonly see like this. `assertEquals(true, sut.doSomething() > 0);`

There are great techniques that can help us address this. TDD is one such technique where we make the test fail first. This allows us to see how the test is going to fail and gives us our first great opportunity to help our future selves. That being said if we don't take advantage of the various assertion libraries available then even detailed feedback could be brittle and misleading. 







 , where we see what a failing test looks like first. This is a great first opportunity to identify missing feedback.  tIf our tests are red it generally means that our system is broken, or that it has deviated from the behaviour that we would describe as "working". Thank goodness we have those tests. It can happen that we look to the test report and our hearts sink at the least helpful feedback we could get.



A couple of questions instantly spring to mind from this feedback: 

1. What was false?
2. Why was it false?
3. why was it supposed to be true? 

The feedback lacks context and bug localisation and both of these mean that we're going to spend time gaining context and potentially debugging. 

We tend to not care too much when they're green.When we write a test it is probably because we want to know when our system is not behaving as we expect.
