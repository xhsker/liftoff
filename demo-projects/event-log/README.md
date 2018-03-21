---
title: 'Demo Project: Event Log'
currentMenu: Demo Projects
---

## Quick Links

- [Assignment Repository for Event Log](https://github.com/chrisbay/liftoff-assignments)
- [Event Log GitHub Repository](https://github.com/chrisbay/event-log)
- [Pivotal Tracker Project](https://www.pivotaltracker.com/n/projects/2157573)
- Jump to week: [1](#week-1) | [2](#week-2) | [3](#week-3) | [4](#week-4) | [5](#week-5) | [6](#week-6) | [7](#week-7) | [8](#week-8)

## Overview

For his Liftoff project, Chris has decided to work on an application movitivated by a need expressed by a nonprofit organization. This page will outline the steps that he follows in developing his project, completing each of the Liftoff assignments along with completing user stories and participating in the weekly agile ceremonies like stand-ups and project kickoffs.

## Week 1

### Sprint 1 Kickoff

Most sprint kickoffs will consist of planning estimating, and committing to user stories to complete during the sprint. Since there are not user stories created yet (we'll do that in week 2) this kickoff is a litle different.

We discussed our project ideas--Chris' for and event log and Paul's for an expense tracker--and got some feedback on how big each project might be to be doable. We also discussed the particular technologies that we'll be using to build our projects.

Finally, we discussed the things that we expect to have to learn along the way, beyond what we already know. Paul will be using a new-to-him framework called Rocket (for the Rust programming language), while Chris has some unknowns around how user authentication will work. He also wants to use [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) which he hasn't done in Spring and will have to learn about.

### Assignment: Project Outline

The [project outline](https://github.com/chrisbay/liftoff-assignments/blob/master/P2-Project_Outline/) for Event Log gives an overview of the desired functionality.

## Week 2

### Sprint 1 Standup

During our sprint 1 standup, we discussed our completed project outlines. We also talked about working on wireframes and writing user stories in the coming week, which seem like very doable and clearly-defined tasks now that we have outlines of our projects.

We also discussed some of the research that we have done around the new skills we'll be learning for our projects. Chris did some resarch on using Spring Security, and will be using that to handle login functionality. Paul has done some general reading about the Rocket framework so he is more familiar with it when he starts coding.

### Assignment: Project Planning

The [Project Planning](https://github.com/chrisbay/liftoff-assignments/blob/master/P3-Project_Planning/) includes 3 initial wireframes for the Event Log app, along with a link to the Pivotal Tracker project that has been populated with some user stories.

Here's a screenshot of the tracker with initial user stories.

![Sprint 1 Stories](images/sprint_1_stories.png)

## Week 3

### Sprint 1 Review and Retrospective

For the spring review/retro Chris and Paul discussed the work completed during the first. Both completed initial project planning and setup. Working through some details such as wireframes and user stories helped clarify the initial work to be done, which will begin in earnest this week as the second sprint kicks off.

During the retrospective portion of the discussion, the discussed how in some ways it didn't feel like much had gotten done since there wasn't much, if any, code written. Paul made the point that while little code was written, the planning that was done should help the initial coding phase of the project go more quickly than it otherwise would. He noted that if a programmer just jumps into a project without designing and planning the work to be done, a lot of time can be wasted in doing things inefficiently, reworking portions of the app, and generally figuring out how it should be structured. Doing this work up front should make things go more smoothly from now on!

### Assignment: Project Setup

[Assignment submission in `liftoff-assignments`](https://github.com/chrisbay/liftoff-assignments/tree/master/P4-Project_Setup)

The project's [GitHub repository](https://github.com/chrisbay/event-log) was set up. Initial commits created a basic "Hello world" Spring Boot app obtained via [start.spring.io](http://start.spring.io/). They also add some basic dependencies in the [`build.gradle`](https://github.com/chrisbay/event-log/blob/3f91742a0527a65e64678c477d50f26a98b87f3e/build.gradle) file for jQuery and Bootstrap (from the `org.webjars` group, as well as a testing database engine, H2.

![Initial commits](images/initial-commits.png)

### Sprint 2 Kickoff

For the second sprint, Chris plans on working through the initial user stories, which are each focused on one aspect of user registration and authentication. He has reviewed the [`spring-filter-based-auth` example](https://github.com/LaunchCodeEducation/spring-filter-based-auth) provided by LaunchCode, and may use that approach. However, he has been learning about Spring Security and wants to see if he can use that framework for setting up registration and login.

He hasn't used Spring Security before, and it looks like it could be complicated to set up. To get started, he's going to refer to the [Spring Security Series](http://www.baeldung.com/security-spring) of articles at [baeldung.com](http://www.baeldung.com/), which seem well-written and thorough. The only challange may be in modifying the complex examples for his more straightforward situation.

He has estimated the user stories that he feels confident he can complete this sprint, and moved them into the **Current Iteration** column of Pivotal Tracker.

![Initial user stories](images/sprint_2_stories.png)

The first story he'll work on will be, "As a user, I can create an account so that I can access the app." The other stories are focused on logging in and out, and one can't log in or out without and existing account, so it makes sense to start this one first. To verify that account registration works, he'll be able to check the database.

Setting up account registration will require him to set up his first model class, `User`, as well as setting up the app's database. Some other initial, one-time work--such as creating some shared template fragments--will also need to be done.

If he completes all of these, there are more stories ready to estimate and begin in the **Backlog**.

## Week 4

### Sprint 2 Standup

## Week 5

### Sprint 2 Review and Retrospective

### Sprint 3 Kickoff

### Assignment: Project Review

## Week 6

### Sprint 3 Standup

## Week 7

### Sprint 3 Review and Retrospective

### Sprint 4 Kickoff

### Assignment: Project Presentation

## Week 8

### Sprint 4 Standup

### Project review

### Optional Assignment: Project Deployment