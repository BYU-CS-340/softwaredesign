# Project Milestone 2 Part A: Layered Architecture
  
For this milestone you will continue to work on the Tweeter UI. The starting point for this milestone is the code you ended up with after completing milestone 1. You are welcome (but not required or expected) to improve the design of your UI, but if you modify it, make sure your user interface includes all views needed to meet the requirements documented in the "Requirements" section of the [Course Project](../project-overview/tweeter.md) overview. **This is to be the front-end of your application only**, so do not make any requests to backend AWS services.  Instead, your front-end application should have realistic dummy data that will later (i.e., in later milestones) be replaced with calls to such backend services.

Although your Tweeter UI should be fully functional, it does not have a proper layered architecture, and contains a lot of duplicated code. Both of these are big problems. Your assignment for this milestone is to refactor the provided code to have a proper layered architecture. In the next assignment (milestone 2B) you will eliminate code duplication--much of which will be created in this milestone (milestone 2A).

To help you, we have created these three videos that explain and demonstrate how to refactoring the "followee" and "follower" functionality into Model-View-Presenter:

1. [Why Refactor Tweeter to MVP](https://youtu.be/ZiGuc0HRnVc) (5:17)
1. [An Explanation of the Code for the UserItemScroller component](https://youtu.be/1p0vNGt8Uvk) (11:49)
1. [An M2A Refactoring Demo](https://youtu.be/S7SSECBlBd0) (1:39:55)

Your task is to apply the changes demonstrated in the video to your milestone 1 code for the "followee" and "follower" functionality as demonstrated in the videos, and then change the rest of your Tweeter UI to use a Model-View-Presenter architecture. The general principle to follow is that any code that can reasonably be pushed out of the UI / Components and Hooks layer into the presenter or model layer, should be pushed out of the UI layer.

**Note:** Learning to work with an unfamiliar code base that you did not write is a critical skill for software engineers. Much of your time will be spent understanding and modifying code written by others.

**Note:** VS Code provides useful refactoring tools under its Refactoring context menu. Using these is optional but highly recommended. VS Code can also be configured with Prettier--a tool that will automatically reformat your code whenever you save a file (if you configure it to reformat on save). This is much easier than reformatting code by hand and will save you time while resulting in better code.

**Note:** As you make your way through the M2A and M2B assignments, frequently build your code (using `npm run build`) and run your code (using `npm start`) to ensure that it is still functioning properly.

## Requirements

Refactor the provided code to have a proper layered architecture. Your refactored design should include:

- A **view** layer consisting of React components and React Hooks. [Already created but will need to be modified]
- A **presenter** layer.
- An **application logic** layer (i.e. your service classes).
- A **domain model** layer. [This already exists in the tweeter-shared module and likely will not need to change]

You are to use the **observer pattern** as demonstrated in the M2A demo video to communicate from the presenter layer to the view layer. The use of async/await prevents the need to use the observer pattern to communicate from the service layer to the presenter layer.

## Detailed Instructions

Your view layer (React components and hooks) should only contain code that directly interacts with the user, either by displaying output or receiving input events from the user. In your milestone 1 code, the view layer also contains the non-visual user interface logic that belongs in the presenter layer, and the "application logic" that belongs in the application-logic layer. Your job is to create presenter and application-logic layers, and move code out of the view layer and into the classes in these new layers.

- Every functionality that contacts the server should follow the layered MVP architecture. There are 14 functionalities total. 12 of them currently call FakeData (either directly or indirectly). PostStatus and Logout do not call FakeData but should also follow the MVP architecture.

### Create an Application-Logic layer

Create a layer of service classes that implement the program's "application logic".  In general, service classes can cache data, implement algorithms, or do whatever is needed to realize the program's non-user interface functionality. For this program, the "application logic" is the code that currently accesses dummy data and will eventually contact the server to send or receive data, and then pass data returned from the server up to the presenter layer.  For this milestone the services will just return the same hard-coded dummy data that is currently accessed from the view layer. In milestone 3, service classes will pass data to and retrieve data from the server.

For this program there are 14 different kinds of requests the client sends to the server. This does not mean you should create 14 different service classes. Rather, you should group related requests into the same service class.  For example, you might have a UserService that handles all user-related requests, a StatusService that handles all status-related requests and a FollowService that handles all follow-related requests.

### Create a Presenter layer

Create a layer of presenter classes for the major components and in some cases hooks in the application. You need a presenter for each component and for each hook that accesses dummy data or has other logic that is not directly related to interacting with the user or rendering the view.

Each presenter class should define a view interface that is used by its component or hook. This is the method interface through which the presenter will call its view. For example, the view interface should include methods for things such as passing data to the view for display, displaying messages to the user, and enabling/disabling UI controls. See the [demo video](https://youtu.be/S7SSECBlBd0) for examples of what this looks like.

Move all user-interface logic that does not directly display output or receive input from each component or hook into the corresponding Presenter classes. Examples of logic you should move from components and hooks to presenters include:

1. User input validation logic
1. Logic for managing paging (e.g., whether there are more pages, what the last retrieved item was, etc.),
1. Code that calls the service classes

The more logic you can move to the presenters the better because presenter code is easier to test than view code and is not limited by restrictions placed on the view code by the view layer framework. However, presenters should never interact directly with the user. If a presenter needs to display a message or otherwise change the visual state of the UI, it should access it's view using the observer pattern. All visual UI logic should go in the view layer, and all non-visual UI logic should go in the presenter layer.

Add public methods to your presenter classes as needed to allow views to call their presenters.

## Passoff

- Pass off your project with a TA by the due date before TA's are off for the day (you must be in the pass off queue 1 hour before they are off to guarantee same-day pass off)
- You can only passoff once.
- If you passoff before the passoff day, you will get an additional 4% of extra credit in this assignment

## Rubric

- (25) Layered Architecture
  - (15) MVP: Generally working, correct logic split among MVP layers
  - (10) Observer Pattern

## [Milestone 2 FAQ](./milestone-2-faq.md)
