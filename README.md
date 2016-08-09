# Screenplan hands-on workshop
**we use java, docker, git, intellij**

We don't like flaky tests, do we? Our tests work, like, all the time. We added some layers and threw some nice design patterns in there, page objects. We love them page objects, those things are awesome.

Now, wait a sec, some guys wrote about the journey pattern, or screenplay pattern. This is yet a new design pattern. We'd better look into that, don't want those flaky tests to bite us in the tail.

Wanna come join us, figure this thing out? Bring programming skills because we are getting hands-on and explore screenplay design pattern in java.

### Preperation

Please make sure you have following software installed on your laptop to participate in the exercises
- java
- git
- docker
- chrome
- intellij

## Section 1: setting the stage
- Why do we need a new design pattern when desiging our test cases?
- What are the different design patterns and introduction into screenplay
- Setup your environment: you need intellij, java, chrome, git, docker

## Section 2: actors and abilities
- Introduction actors and abilities
- Hands-on exercise

## Section 3: tasks and actions
- Introduction tasks and actions
- Hands-on exercise

## Section 4: all together now
- Bringing all the pieces together
- Hands-on exercise

## How to run this presentation

```
docker run -ti -d \
-p 8989:80 \
-v $(pwd):/usr/share/nginx/html:ro \
nginx
```
