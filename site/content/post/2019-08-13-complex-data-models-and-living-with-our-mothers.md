---
templateKey: blog-post
title: Complex data models and living with our mothers
date: 2019-08-13T14:04:10.000Z
description: >-
  Engineering software is hard and it's even harder when you don't have the
  freedom and confidence to make major changes to your application. Even those
  of us that might have amazing automation protection could find themselves in
  the awful position where a change could turn your tests from friends to foes
  as they all go red. There are patterns that can help keep your test suites
  friendly. I want to introduce you to the 'mother of all creation methods',
  Object Mother. 
featuredpost: true
featuredimage: /img/mothersday.jpg
tags:
  - Object Mother
  - Testing
  - Engineering
  - Quality
  - Agility
  - Robustness
  - Resilience
---
![Happy mothers day note with flowers](/img/mothersday.jpg)

- - -

## What do we want?

Whatever software we are writing, we want the associated tests to exhibit certain properties.

### Easy to write

Tests that are difficult to setup and write will probably not get written. If we need to spend time figuring out and recreating our model for every test then we're probably violating DRY (Don't Repeat Yourself) and we're also likely making mistakes. 

_We want the test we're creating to be a **delight to write**._

### Easy to understand

If we've rewritten the setup enough times, or over used the same [Creation Method](http://xunitpatterns.com/Creation%20Method.html) where it now has X+1 parameters, then the important data in the test is potentially lost in the noise. 

_We want the test we've created to be a **delight to read**._

### Resistant to change

It is a terrible irony of software that we know the most about a problem when we are done solving it, and therefore we can be confident that we will change our context and our model, sometimes significantly depending on the pivot. 

_We want our tests **to delight us** as they prove our successful refactor or redesign._

## Object Mothers Arrange

Object Mother is a [Test Helper](http://xunitpatterns.com/Test%20Helper.html) that is used to birth your model with reasonable and potentially a well described variety of data. It is a [creational pattern](https://en.wikipedia.org/wiki/Creational_pattern) for objects with "canned" data and comes into its own when your model is significantly complex and is used with other creational patterns like [Factory](https://en.wikipedia.org/wiki/Factory_method_pattern) or [Builder](https://en.wikipedia.org/wiki/Builder_pattern). 

\[insert model uml]

For a reasonably complex model we could have a setup that hides the most important data to the test, is difficult to read, and is difficult to change and maintain. 

```java
@Test
void willApproveMortgage_WhenMoreAssets_ThanDebts() {  
  Application application = new Application();
  Applicant applicant = new Applicant();  
  applicant.setName("John");  
  application.set(applicant);  
  Employer job = new Employer();  
  job.setRole("programmer");  
  job.setCompany("Delightful Software");  
  job.setSalary(new Money("100"));  
  applicant.setEmployer(job);  
  applicant.setDebts(List.of(new Debt(new Money("50")))); 
  applicant.setAssets(List.of(new Asset(new Money("60"))));  
  application.setRequestedAmount(new Money(100));
  ...
 }
```

None of the above code is delightful, so let's introduce our mothers. 

### Factory Mothers

```java
@Test
void willApproveMortgage_WhenMoreAssets_ThanDebts() { 
  Money debt = new Money("50"); 
  Money asset = new Money("60");
  Application application = ApplicationObjectMother.getApplicationForJohn(asset, debt); 
  ...
 }
```

These can be an excellent entry level Object Mother and a natural evolution of the [Creation Method](http://xunitpatterns.com/Creation%20Method.html). We can setup our data in a convenient place with a [reasonable description or name](http://xunitpatterns.com/Creation%20Method.html#Named%20State%20Reaching%20Method) and allow some variety in the objects being created. There are some draw backs to the factory as it becomes tiresome to add variety or additional options to customize your creation.  

### Builder Mothers

```java
@Test
void willApproveMortgage_WhenMoreAssets_ThanDebts() {
  Application application = ApplicationObjectMother.forJohn()
            .withAssets(new Money("60"))
            .withDebts(new Money("50"))
            .build(); 
  ...
 }
```

It is my personal preference to use Builder with a [Fluent Interface](https://en.wikipedia.org/wiki/Fluent_interface) for any particularly gnarly model. It offers the greatest flexibility for creation and you can get very creative around the your named state e.g. 

```java
  ApplicationObjectMother.forJohn()
                         .afterRedundancyPackage(new Money("100"))
                         .build();
```

## Benefits

By using an Object Mother it allows us to apply many of the patterns that make a difference to the properties we want from our tests. We ensure accuracy in the creation of the model that we're testing. The data that is important to the sut for the specific test case is concise and clear. The language around your data setup can be easily shared with your business visionaries, testers, and potentially as personas from UX. 

**_When_** your model changes in the future, your tests can often be protected from them by your Object Mothers. Where a state of your data has simply changed shape, the mothers can expose the same interfaces and therefore make your tests resilient to some fairly significant movement. 

## Draw backs

Coupling is an obvious drawback because suddenly any test that needs an `Application` for John is coupled to the `ApplicationObjectMother`. This is a trade-off to avoid being WET (Write Everything Twice).  There is a significant risk of introducing a [Mystery Guest](http://xunitpatterns.com/Obscure%20Test.html#Mystery%20Guest) as a test smell through hiding important data to your test in your Object Mothers.

## Wrapping it up for our mothers

If the Arrange of your tests is not a delight, then introducing the Object Mother might be something to start getting your test setup under control. For those of us that are lucky enough to be working on a project early in its life, [@GeePawHill](https://twitter.com/GeePawHill) gives us some great advice, perhaps you might consider ObjectMother as your builder.

 [![tdd pro-tip #6: prevent complex test data from spiraling out of control by going to builder & custom comparator early on.](/img/geepawhill-twitter-builder-comparators.png "GeePaw Hill on planning for your system to become more complex early")](https://twitter.com/GeePawHill/status/1043228698512695296)



## References

![Book cover: xUnit Test Patterns - Refactoring Test Code](/img/xunit-test-patterns.gif)

* [Meszaros, G. (2010). XUnit test patterns: refactoring test code.](http://xunitpatterns.com/)
* [Martin Fowler on Object Mother](https://martinfowler.com/bliki/ObjectMother.html)

## Images

[Photo by Giftpundits.com from Pexels](https://www.pexels.com/photo/happy-mothers-day-card-beside-pen-macaroons-flowers-and-box-near-coffee-cup-with-saucer-2072160/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)
