---
title: Logging in Java Applications
currentMenu: articles
---

In this tutorial, we'll discuss why logging is important, how to control your logging output, how to write your own logging statements, and how to view your logs in production.

- [Introduction](#introduction)
- [Setup](#setup)
- [Reading Log Messages](#reading-log-messages)
- [Controlling Logging Output](#controlling-logging-output)
- [Adding a Logger](#adding-a-logger)
- [Passing Objects](#passing-objects)
- [Reading Logs in Cloud Foundry](#reading-logs-in-cloud-foundry)
- [Reading Logs in Pivotal Web Services](#reading-logs-in-pivotal-web-services)
- [Additional Resources](#additional-resources)

## Introduction

### Requirements

This tutorial assumes you have the following installed:

- [Spring Boot](http://projects.spring.io/spring-boot/)
- [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
- [Gradle](https://gradle.org/install/)

### Why Logging Matters

- Logging provides observability of your app in a live environment. What went wrong? What does the stack trace of an error experienced in production look like?
- Logging allows you to debug errors.
- Using a logging framework provides a consistent, informative, timestamped output, and handles the formatting and display of stack traces. If you've used `System.out.println` to output information, you've noticed that these messages lack that extra context that the other messages have.

### Considerations

- Don't log sensitive information, such as passwords, PII (personally identifiable information), or credit card numbers.
- Too much logging is hard to sort through. Too little logging won't provide insight into critical issues. We'll talk about a few techniques to manage the "signal to noise" ratio inherent in getting this right.

## Setup

Spring Boot by default ships with all the necessary libraries to add additional logging to your application. You will be interacting with [SLF4J](https://www.slf4j.org/), a logging abstraction which simplifies logger use. Under the hood, the logging implementation is provided by [Logback](https://logback.qos.ch/).

This tutorial will be based off of the [cheese-mvc](https://github.com/LaunchCodeEducation/cheese-mvc/tree/video-validation-end) project. Clone this project and check out the 'video-validation-end' branch as a new branch called `logging` (`git checkout -b logging` from the `video-validation-end` branch). If you'd like to keep the changes we'll be making, first fork the project to your own GitHub account.

Feel free to use your own project for this tutorial, just make sure you update class and package names where relevant.

## Reading Log Messages

You likely already have some experience reading log messages, but let's examine a typical event logged by Spring.

Here's the first log message that displays immediately under the Spring Boot banner on startup:

`2017-11-05 13:33:58.594  INFO 73394 --- [  restartedMain] org.launchcode.CheeseMvcApplication      : Starting CheeseMvcApplication on LaunchCodeComputer with PID 73394 (/Users/launchcoder/workspace/cheese-mvc/build/classes/main started by launchcoder in /Users/launchcoder/workspace/cheese-mvc)`

Here's what's displayed by default:

* Date and Timestamp
* Log Level — ERROR, WARN, INFO, DEBUG or TRACE (discussed next)
* Process ID
* Thread name — Enclosed in square brackets (may be truncated for console output).
* Logger name — This is usually the source class name (often abbreviated).
* The log message - Here Spring is telling us what process ID our application started on, where it was started from, and by whom.

## Controlling Logging Output

Loggers typically support five different log levels, or degrees of severity, ranging from most severe (`ERROR`) to least severe (`TRACE`). As a developer, this provides context for the message (How serious is the issue?), and enables control over which messages are displayed during runtime.

The five standard log levels, in order of increasing severity, are:

- `TRACE`
- `DEBUG`
- `INFO`
- `WARN`
- `ERROR`

Each message that is potentially logged by an application has a level, and a running application has a level configured as well. The application log level controls which messages are actually logged, with all messages at the applications log level or higher being logged. For example, if the application level is set to `INFO`, then `INFO`, `WARN`, and `ERROR` messages are displayed. If the application level is `ERROR`, then only messages with that level are displayed. If the application level is `TRACE`, then all messages are displayed in the logs.

Logging levels allow you to control what information an application logs. You may want to only log `INFO` and above when your application is working normally, but you may want to enable `DEBUG`-level logs in order to troubleshoot a problem without redeploying your code. Logging frameworks allow you to control the log level via configuration files and properties.

In Spring Boot projects, you can use the `application.properties` file to control log levels. Spring allows log levels to be set not just for the application as a whole, but also separately for individual packages. This allows different packages to have different levels. This feature can help you focus in on only the most important messages. For example, if you are debugging a problem and know precisely which package the issue lies in, you can reduce the number of messages that you have to wade through by enabling more verbose logging for just that package.

Within `application.properties`, all properties are prepended with `logging.level`, followed by the package name and a log level. Spring scans the file for such properties on application startup.

For example, if you want to log every event at a specific level use:

```nohighlight
logging.level.root=DEBUG
```

If you want to log everything that Spring is doing, you'll set:

```nohighlight
logging.level.org.springframework=TRACE
```

Notice that we're setting the log level here only for the package `org.springframework`. Any `DEBUG` message emitted by a class at or below this package/directory will be displayed.

Say you want to log everything in your app, but ignore informational messages from Spring. You would set the following properties:

```nohighlight
logging.level.org.springframework=WARN
logging.level.org.launchcode=TRACE
```

Here are some more common packages that may be useful to log during debugging:

```nohighlight
# For debugging security
logging.level.org.springframework.security=DEBUG

# General debugging of web applications without getting ALL of Spring's logs
logging.level.org.springframework.web=DEBUG

# Database/ORM logging. Best used in conjunction:
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.type=TRACE

# Consuming REST apis via RestTemplate:
logging.level.org.springframework.web.client.RestTemplate = debug
logging.level.org.apache.http = debug
```

<aside class="aside-note" markdown="1">
Some loggers also provide a `FATAL` level, but logback treats this the same as an `ERROR`.
</aside>

## Adding a Logger

As you start to build more complex applications and host them in a live environment, you'll want to start adding your own logging messages.

To do so, you'll need to instantiate (create) a logger inside the class we want to do some logging in. This is done by adding the following to the top of your class, inside of your class declaration.

```java
private static final Logger logger = LoggerFactory.getLogger(CheeseController.class);
```

Here, we're adding a logger to our `CheeseController`, but if you're working on your own project, be sure to use the appropriate class name.

This gives us access to a logging instance, which we can start using to create log messages.

## Creating a log statement

Consider a situation where you might want to log why a user failed validation. This way, if the user contacts us confused as to why they can't create a new cheese, register, or otherwise use our application, we'll have a record of what was the root problem. Add the following line inside one of our validation blocks, like so:

```java
if (errors.hasErrors()) {
    logger.info("Error during cheese registration: " + errors.getAllErrors().toString());
    model.addAttribute("title", "Add Cheese");
    return "cheese/add";
}
```

In the event that a user has provided incorrect information when adding a cheese, we will log that there was an error, as well as the errors themselves, which should give us insight into what was the root of the problem.

## Passing Objects

Here we are just logging a string which we concatenate with the `+` symbol. Each logger provides many overloaded methods allowing you to pass in additional parameters, such as an object or an exception. Any objects that are passed in will be logged via their `toString` method. If it's an object you've created, make sure you've overridden the default `toString` and you aren't logging any sensitive information stored in your object.

## Reading Logs in Cloud Foundry

If you've deployed your app via Cloud Foundry, you can view your logs by using the `cf logs` command. For example, if your app was named `cheese-mvc` you'd type `cf logs cheese-mvc`. This will display your logs in real time. If you want to view recent logs, add the `--recent` flag like so `cf logs cheese-mvc --recent` and the most recent logs will be displayed.

## Reading Logs in Pivotal Web Services

If you've hosted your app using Pivotal Web Services, you can also view your logs by logging into your [PWS console](console.run.pivotal.io/). Navigate to your space, then your application, and there should be a tab for "Logs". From there, you can view old logs, as well as monitor them in real time.

## Additional Resources

* [Spring Boot Docs - Logging Features](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-logging.html)
* [Spring Boot Docs - Logging How To](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html)
* [Cloud Foundry Logging](https://docs.cloudfoundry.org/devguide/deploy-apps/streaming-logs.html)
