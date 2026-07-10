# Course Project: Tweeter

## [Tweeter Demo Video](https://www.youtube.com/embed/mdpHjeYCYtM)

## Overview

In this project you will implement a social media application somewhat like a simplified version of Twitter or Instagram.

The project is divided into milestones. In the first few milestones, you will modify an existing web client that is provided for you. Although the provided client works, it uses dummy data and it does not use the design and implementation concepts you will learn in class. Your task for these early milestones will be to improve the design and structure of the provided UI by applying concepts learned in class.

In later milestones you will replace the use of dummy data with a backend implementation on AWS. 

## Milestones

- Milestone 1: [Tweeter Web UI Cleanup](../milestone-1/milestone-1.md)

- Milestone 2: Working UI with Dummy Data
    - [Part A: Layered Architecture](../milestone-2a/milestone-2a.md)
    - [Part B: Remove Code Duplication](../milestone-2b/milestone-2b.md)
    - [Part C: Automated Testing and Milestone Report](../milestone-2c/milestone-2c.md)

- Milestone 3: [API Design and Implementation](../milestone-3/milestone-3.md)

- Milestone 4: Complete Scalable Back-end with Persistence Layer
    - [Part A: Data Access Layer](../milestone-4a/milestone-4a.md)
    - [Part B: Scalable Status Processing](../milestone-4b/milestone-4b.md)

## Domain Concepts

**Your application will be based on the following domain concepts:**

- **User** of the system has the following attributes
    - Name: the user’s real name (eg. Matt Pope).
    - Alias: the user’s login ID or handle (eg. matt).
    - Photo: a picture of the user.
- A **status** (ie, message) is a publicly readable, limited-length character string, which can contain 
    - User mentions which is a user alias preceded by the “@” symbol (eg, @matt)
    - URLs
- A user’s **story** is all of the statuses posted by that user.
- **Follows** is an asymmetrical relationship between users, meaning user A can follow user B without user B following user A. We refer to all of the users following user A as user A’s followers. We refer to all of the users A follows as A’s following.
- A user’s **feed** is all of the statuses posted by users he or she follows, sorted from newest to oldest. A detail to note here is that a status is only included in user’s feed if the status was posted at the time the user was following the author.

## User interface

**Here are a few general user interface considerations, spanning multiple application views.**

1. All of the main views are paginated lists of statuses or users. By paginated we mean that the content is loaded a “page” at a time. The application will add an additional page of content when the user of the app reaches the bottom of the page or using something like a “more” button in the user interface.
1. All displays of statuses turn user mentions (@...) and URLs into clickable links. A user mention links to a user story view (which page should facilitate access to that user’s statuses, followers and following).
1. All displays of statuses include the profile image of the author of the status
1. Across all views, actions that are impossible should be disabled or not included in the user interface at all. For example, follow or unfollow functionality only make sense when a user is logged in and depends on whether the currently logged in user is (or is not) already following a given user.
1. Errors should be handled and (when appropriate) communicated to users in a consistent way.

## Requirements

For milestone 2 to milestone 4 you will be implementing a core set of features for the application. More details on what is expected for each milestone will be shared in a separate document. Note that in the following when we state *“A signed in user can…”* we are implying that only a signed in user can perform that action. Similarly, when we state *“A user can…”* we mean that a user can perform that action whether or not they are signed in.

**Your application is to satisfy the following user and session management requirements:**

1. A new user can sign up for the service, specifying their name, alias, and password, and providing an image to upload (i.e., their profile photo). After signing up a user is automatically “signed in” and is redirected to their (at this point) blank feed.
    1. User passwords are to be stored as hashes
    1. User profile pictures are stored on AWS S3.
1. A user that has previously signed up for the service, can sign in (ie. authenticate) by supplying their alias and password. If the alias and password are correct the user is redirected to her or his feed.
    1. Authentication tokens are to expire after N minutes of inactivity.
1. A signed in user can sign out (ie log off) of the service.

**And status related requirements:**

1. A signed in user can post a status. The system will then add that newly created status to the feeds of all of the author’s followers.
1. A signed in user can view all statuses of the users they follow merged into one list, sorted from newest to oldest. We call this list of statuses the user’s feed. It should be the default view of the application for a signed in user.
1. A user can view all statuses posted by a given user, sorted from newest to oldest. We call this list of statuses a user’s story. It can be reached by clicking on a “mention” link (@…).

**And following/followers related requirements:**

1. A user can view all of the users followed by a particular user. Naturally this includes the ability for a signed in user to see all of the users she or he follows.
1. A user can view all of the users following a particular user. Again, this includes the ability for a signed in user to see all users following him or her.
1. A user can see a count of all of his or her followers and following.
1. A signed in user can follow an unlimited number of other users, though a user cannot follow herself or himself or someone he or she is already following.
1. A signed in user can only unfollow users they are following. After user A unfollows user B, user B’s new statuses are no longer added to A’s feed, however all of B’s statuses that had previously been added to A’s feed remain in A’s feed.
1. A user can see a count of how many users are followed by a particular user. The same goes for the count of how many users are following that particular user. Naturally, these counts should change when users select to follow or unfollow another user.

## Domain Model

This UML domain model shows how these concepts relate to each other.

![Tweeter Domain Model](tweeter-domain-model.png)
