
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
# Why screenplay?

!SUB
# Why design patterns?

Design patterns are solutions to general problems that software developers faced during software development.

Design patterns provide a standard terminology and are specific to particular scenario.


!NOTE

Design patterns represent the best practices used by experienced object-oriented software developers.<br />
For example, a PageObject design pattern signifies use of single object with ui specific code so all developers familiar with page object design pattern will make use of this page object and they can tell each other that program is following a Pageobject pattern.

!SUB
## Page-objects

* PageObjects are the standard for automated web testing for many years.
* Simon Stewart wrote the original Selenium PageObject wiki entry in 2009
* Test suites become slow, fragile and unreliable
* Code smell and coding principles

!NOTE
Little history about PageObject pattern. It's a industry standard. There are some known problems with it. Although some suggests this is due to bad test design. Another concern is the pageobject pattern does not adhere to all coding principles and does contain some code smell.


!SUB
## Page object code smell

> A code smell is a surface indication that usually corresponds to a deeper problem in the system. The term was first coined by Kent Beck while helping me with my Refactoring book.

*- Martin Fowler* <!-- .element: class="author" -->

!NOTE

For example > Large class
- takes longer to understand
- a solution would to split up in

!SUB
## Coding principles

SOLID is an acronym coined by Michael Feathers and Bob Martin that encapsulates five good object-oriented programming principles

* Single Responsibility Principle <!-- .element: class="fragment" data-fragment-index="1" -->
* Open Closed Principle <!-- .element: class="fragment" data-fragment-index="1" -->

We’ll concentrate on the two that have the most noticeable effect on refactoring of PageObjects — the Single Responsibility Principle (SRP) and the Open Closed Principle (OCP). <!-- .element: class="fragment" data-fragment-index="2" -->

!NOTE
S: a class should have only a single responsibility (i.e. only one potential change in the software's specification should be able to affect the specification of the class)
O: “software entities … should be open for extension, but closed for modification.”
L: “objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.”
I: “many client-specific interfaces are better than one general-purpose interface.”
D: one should “Depend upon Abstractions. Do not depend upon concretions.
https://en.wikipedia.org/wiki/SOLID_%28object-oriented_design%29


!SUB
## SRP & OCP
Single Responsibility Principle: The SRP states that a class should have only **one** responsibility and therefore only one reason to change.
* Easier to test, read and maintain
* Separation of Concerns

Open Closed Principle: To satisfy OCP it should be possible to simply add a new class that describes how to sort the list.
* Prevents from bugs creeping in

!SUB
## Page objects commonly have the following responsibilities

* Provide an abstraction to the location of elements on a page via a meaningful label for what those elements mean in business terms. <!-- .element: class="fragment" data-fragment-index="1" -->

* Describe the tasks that can be completed on a page using its elements(often, but not always, expressing navigation in the PageObject returned by a task). <!-- .element: class="fragment" data-fragment-index="2" -->

!NOTE
Page objects provide abstraction of elements on a page and describe task that can be performed. This violating the single responsibility principle. We will see an practical examples in the hands-on exercises later. New tests typically need modifications to existing Page Object classes, introducing the risk of bugs.


!SUB
# Page-objects are not user stories

**Behaviours** are the primary concern of our tests, the **implementation** a secondary concern.

!NOTE
By starting from this point of view, this changes the way we perceive the domain and therefore how we model it. This thinking takes us **away from pages**. Instead, we find we have **actors** with the **abilities** to play a given role. Each test scenario becomes a narrative describing the tasks and how we expect a story to play out for a given goal.


!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Getting Started Screenplay


!SUB
# It Starts with user story

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

* **Tests** describe how a user interacts to _achieve a goal_
* A _user interacting_ with the system is an **Actor**
* **Actors** are at the _heart of the Screenplay pattern_
* Each **Actor** _has_ one or more **Abilities**
* **Actors** can also _perform_ **Tasks**
* To achieve these **Tasks**, they will typically need to _interact with the application_. We call these interactions **Actions**
* **Actors** can also _ask_ **Questions** about the _state_ of the application

!NOTE
For this reason, tests read much better if they are presented from the point of view of the user (rather than from the point of ‘pages’).

!SUB
## Screenplay actor centric model

![Screenplay](img/screenplay.jpg)

*screenplay domain modal*

!SUB
# Set up

* load the maven project into IntelliJ
  * *File > New > Project from Existing Sources*
  * ~/academy-day/screenplay-meetup/
* checkout the *master* branch
* fix the credentials.properties file (src/test/resources)
  * Create a meetup.com account if you don't have one (don't login with google or facebook!)
  * Join the Test Masters Series group (https://www.meetup.com/Test-Masters-Series/)
  * Add your credentials to credentials.properties.example
  * Rename the file to credentials.properties
* ```bash
$ mvn clean verify -Dtags=PageObjects
```

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Testing the Meetup.com website

!SUB
#Features

* Login to the website
* Message another member in a meetup group

!SUB
#Page Object implementation

Let's take a look together.

!SUB
# Refactor to the screenplay pattern

* Actors & Abilities
* Tasks & Actions
* Questions

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Actors & Abilities

!SUB
# As a Test Master ...

A _user interacting_ with the system is an **Actor**

```java
Actor tim = new Actor.named("Tim");
```

* Give the actor a name
* .. a name that you can easily associate with the actor's role
  * Sales person -> Sally

!NOTE
Think of the actor as a physical human.

!SUB
# Ability to Browse the Web

Each **Actor** _has_ one or more **Abilities**

```java
tim.can(BrowseTheWeb.with(hisBrowser));
```

* BrowseTheWeb ability is build into Serenity
* Manages the WebDriver instance

!NOTE
Think of the actor now having an object to interact with, a computer with a browser.

!SUB
# Define your own abilities

```java
public class MyAbility implements Ability {}
```

Examples:
* Ability to use an API
* Ability to Authenticate (with a specific Role?)
* Ability to load a data file

!NOTE
This gives you the possibility to manage source code needed to make available interfaces for interactions.
Or define specific roles.

!SUB
# Hands-on: Ability to Authenticate

```java
tim.can(Authenticate.withCredentials("username","password"));
```

* ```bash
$ git checkout exercise1
```
* complete the **Authenticate** implementation
* ```bash
$ mvn clean verify -Dtags=Screenplay
```

!NOTE
Hints:<br/>
Create a new Authenticate object within within the withCredentials function.

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Tasks & Actions

!SUB
# Abstract example

**Actors** can also _perform_ **Tasks**

Abstract scenario:
```
Given actor was able to perform a task
When actor attempts to perform another task
Then actor...
```

* In scenarios actors perform tasks

!SUB
# Concrete example

Log in scenario:
```
Given Tim was able to open the login page
When Tim attempts to login with his credentials
Then Tim ...
```

* Our actor "Tim" performs specific tasks:
  * "Open the login page"
  * "Login with his credentials"

!SUB
# Implemented example

```java
givenThat(tim).wasAbleTo(openTheLoginPage);
when(tim).attemptsTo(Login.withCredentials());
then(tim)...;
```

* Tasks are simple references to "Performable" classes
* Implementation reads as a scenario

!NOTE
Here some wrapper functions are introduced also:<br/>
<br/>
givenThat, when, then:<br/>
These wrap actor objects<br/>
<br/>
wasAbleTo, attemptsTo:<br/>
These wrap Performable objects (Tasks, Actions)<br/>
<br/>
See also: http://thucydides.info/docs/serenity-staging/#_actors_perform_tasks

!SUB
# Task class layout

```java
public class MyTask implements Task {
  @Override
  public <T extends Actor> void performAs(T actor) {
    // Here the actor performs the actions to complete the task
  }
}
```

* A task is implemented as the actor performing one or more _Performables_
  * other tasks
  * or actions

!SUB
# Actions

* Also "Performable" classes
* Used to perform interactions with the application.

Examples of build-in WebDriver actions:
* Open -> to open a browser on a page
* Enter -> to enter text into a field
* Click -> to click on a button or link

!SUB
# Hands-on: Open the website & Log in

* ```bash
$ git checkout exercise2
```
* complete the classes in the *tasks* package
* use the targets defined in the page objects in the *ui* package
* ```bash
$ mvn clean verify -Dtags=Screenplay
```

!NOTE
Hints:<br/>
The Target for the BrowseToTheLoginPage Task is in the MeetUpLandingPage PageObject.<br/>
The value for the password field can be retrieved using the authenticated method, like for the username.<br/>
The Targets for the LogIn Task are in the LoginPage PageObject.<br/>
To LogOut you need to perform two actions. Click the HeaderNavigation dropdown, and then the logout link.

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Questions

!SUB
# Finish the scenario

```
Given actor was able to perform a task
When actor attempts to perform another task
Then actor should see that there is some new state
```

After the actor has performed the tasks
the **actor** can ask **questions** about the **state** of the application

!SUB
# Is the actor logged in?

How do we know the actor is logged in?

Meetup.com example:
* The header contains the users avatar

```java
givenThat(tim).wasAbleTo(openTheLoginPage);
when(tim).attemptsTo(Login.withCredentials());
then(tim).should(seeThat(theUserAvatarIsVisible));
```

!NOTE
The should(seeThat(...)) wrapper functions use a Hamcrest assertion to validate the state.

!SUB
# Question class layout

```java
public class MyQuestion implements Question<String> {
    @Override
    public String answeredBy(Actor actor) {
        return "Some string answer";
    }
}
```

* Question implementations are of a certain type (String, Boolean, Integer etc.)
* The answerBy method should return the same type
* Serenity has build-in functionality to check elements via WebDriver

!SUB
# Hands-on: Avatar Visible?

* ```bash
git checkout exercise3
```
* complete the class in the *questions* package
* ```bash
mvn clean verify -Dtags=Screenplay
```

!NOTE
Hints:<br/>
Serenity has a class CurrentVisibility<br/>
Which can view a target as an Actor<br/>
And return a Boolean

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# All together now...

!SUB
# Hands-on: Messaging feature

* ```bash
git checkout exercise4
```
* classes to complete:
  * tasks.BrowseToTheMessagesPage
  * tasks.messaging.DraftANewMessage
  * tasks.messaging.SendTheMessage
  * questions.MostRecentConversationPartner
* ```bash
mvn clean verify -Dtags=Screenplay
```

!NOTE
Hints:<br/>
To Draft a new message:<br/>
*- Click the new message button*<br/>
*- Enter the recipient username*<br/>
*- Click the found member*<br/>
*- Enter the message text*<br/>
For the question, you can user the Text class.<br/>
```bash
git checkout finished
```

!SLIDE
<!-- .slide: data-background="#6B205E" -->
# Wrap-up

!SUB
# Pro's & Con's

Will you be using this pattern?
