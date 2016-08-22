
<!-- .slide: data-background="#6B205E" -->
<center><div style="width: 65%; height: auto;"><img src="img/xebia.svg"/></div></center>
<br />
<center>
<br />
<br />
# Workshop Webdriver Patterns
## Page-objects to Screenplay

**Maarten van den Ende** mvandenende@xebia.com <br />
**Jochum Börger** jborger@xebia.com

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Structure

- Why screenplay
- Screenplay getting started
- Actors & Abilities
- Tasks & Actions
- Questions
- All together now
- Wrap-up

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Why screenplay

Design patterns are solutions to general problems that software developers faced during software development. <!-- .element: class="fragment" data-fragment-index="1" -->

Design patterns provide a standard terminology and are specific to particular scenario. <!-- .element: class="fragment" data-fragment-index="2" -->


!NOTE

Design patterns represent the best practices used by experienced object-oriented software developers. Design patterns are solutions to general problems that software developers faced during software development. These solutions were obtained by trial and error by numerous software developers over quite a substantial period of time.

For example, a singleton design pattern signifies use of single object so all developers familiar with single design pattern will make use of single object and they can tell each other that program is following a singleton pattern.

!SUB
## Page-objects

* PageObjects are the standard for automated web testing for many years.
* Simon Stewart wrote the original Selenium PageObject wiki entry in 2009
* Page-objects are not user stories
* Code smell and coding principles
* Test suites become slow, fragile and unreliable

!SUB
# Page-objects are not user stories

* **Behaviours** are the primary concern of our tests; the **implementation** — a secondary concern.

* By starting from this point of view, this changes the way we perceive the domain and therefore how we model it. This thinking takes us **away from pages**. Instead, we find we have **actors** with the **abilities** to play a given role. Each test scenario becomes a narrative describing the tasks and how we expect a story to play out for a given goal.

!SUB
## Page object code smell

> A code smell is a surface indication that usually corresponds to a deeper problem in the system. The term was first coined by Kent Beck while helping me with my Refactoring book.

  *- Martin Fowler* <!-- .element: class="author" -->

* Large class

!SUB
## Coding principles

SOLID is an acronym coined by Michael Feathers and Bob Martin that encapsulates five good object-oriented programming principles

* Single Responsibility Principle <!-- .element: class="fragment" data-fragment-index="1" -->
* Open Closed Principle <!-- .element: class="fragment" data-fragment-index="2" -->
* Liskov Substitution Principle <!-- .element: class="fragment" data-fragment-index="3" -->
* Interface Segregation Principle <!-- .element: class="fragment" data-fragment-index="4" -->
* Dependency Inversion Principle <!-- .element: class="fragment" data-fragment-index="5" -->

We’ll concentrate on the two that have the most noticeable effect on refactoring of PageObjects — the Single Responsibility Principle (SRP) and the Open Closed Principle (OCP). <!-- .element: class="fragment" data-fragment-index="6" -->

!SUB
## SRP - Single Responsibility Principle
The SRP states that a class should have only one responsibility and therefore only one reason to change. This reduces the risk of us affecting other unrelated behaviours when we make a necessary change.

> If a class has more than one responsibility, then the responsibilities become coupled. Changes to one responsibility may impair or inhibit the class’ ability to meet the others. This kind of coupling leads to fragile designs that break in unexpected ways when changed.

*- Robert Martin, Agile Principles, Patterns & Practices*

!SUB
## SRP: PageObjects commonly have the following responsibilities:

Provide an abstraction to the location of elements on a page via a meaningful label for what those elements mean in business terms. <!-- .element: class="fragment" data-fragment-index="1" -->

Describe the tasks that can be completed on a page using its elements(often, but not always, expressing navigation in the PageObject returned by a task). <!-- .element: class="fragment" data-fragment-index="2" -->

!SUB
## SRP: Simple example

Create example

!NOTE

show structure of simple page, login or meetup page?
* Elements: toggleAllButtons, filterTodos, newTodo
* Tasks: addATodoItem, filterItems, ToggleAllCompleted

!SUB
## OCP - Open Closed Principle

The Open Closed Principle (coined by Bertrand Meyer in Object-Oriented Software Construction) states that a class should be open for extension, but closed for modification.

> Adding a new feature would involve leaving the old code in place and only deploying the new code, perhaps in a new jar or dll or gem. 

*— Robert Martin, The Open Closed Principle*

!SUB
## OCP: Simple example

To satisfy OCP it should be possible to simply add a new class that describes how to sort the list.

Scenario: add ability to sort items on popularity

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Screenplay getting started

- Start with user story

```
Should be able to login with credentials

As Anna
I want to be able to view the website as an authorized user
So that I can see my avatar
```

!SUB
## Test scenario
```java
@Test
public void should_be_able_to_login_with_credentials() {
  givenThat(anna).wasAbleTo(openTheMeetUpWebsite);
  when(anna).wasAbleTo(browseToTheLoginPage);
    and(anna).attemptsTo(LogIn.withCredentials());
  then(anna).should(eventually(seeThat(theUserAvatarIsVisible)));
}
```

!SUB
## Screenplay domain

* Tests describe how a user interacts to achieve a goal
* A user interacting with the system an Actor
* Actors are at the heart of the Screenplay pattern
* Each actor has one or more Abilities
* Actors can also perform Tasks
* To achieve these tasks, they will typically need to interact with the application. We call these interactions Actions
* Actors can also ask Questions about the state of the application

!NOTE
For this reason, tests read much better if they are presented from the point of view of the user (rather than from the point of ‘pages’).

!SUB
## Screenplay actor centric model

![Screenplay](img/screenplay.jpg)

*screenplay domain modal*

!SUB
# Set up

* git clone https://github.com/xebia/screenplay-meetup.git
* load the maven project into IntelliJ
* fix the credentials.properties file (src/test/resources)
* mvn verify -Dtags=PageObjects

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Actors & Abilities

!SUB
# As a Test Master ...

```java
Actor tim = new Actor.named("Tim");
```

* Give the actor a name
* .. a name that you can easily associate with the actor's role
  * Sales person -> Sally

!SUB
# Ability to Browse the Web

```java
tim.can(BrowseTheWeb.with(hisBrowser));
```

* BrowseTheWeb ability is build into Serenity
* Manages the WebDriver instance

!SUB
# Define your own abilities

```java
public class MyAbility implements Ability {}
```

Examples:
* Ability to use an API
* Ability to Authenticate (with a specific Role?)
* Ability to load a data file

!SUB
# Hands-on: Ability to Authenticate

```java
tim.can(Authenticate.withCredentials("username","password"));
```

* git checkout exercise1
* complete the Authenticate implementation
* mvn verify -Dtags=Screenplay

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Tasks & Actions

!SUB
# Example

!SUB
# Hands-on: Open the website

* Implement 'Task' interface
* Use 'Open' action

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Questions

!SUB
# Hands-on: Avatar Visible?

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# All together now...

!SUB
# Hands-on: Messaging feature

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Wrap-up

!SUB
# Pro's & Con's
