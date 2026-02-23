# Project Milestone 1: Tweeter Web UI Cleanup

Before starting this milestone, you should review the description and domain model in the [course project overview](../project-overview/tweeter.md).

Your assignment for this milestone is to cleanup the structure and design of the provided Tweeter UI by adding React components and React hooks. The assignment is divided into three parts.

## Part 1: React Components

In part one of this assignment, you will improve the provided Tweeter UI by adding react components. This [demo video](https://youtu.be/KSm7dK0mYM4) shows you how to create React components by creating two components for you. After watching the video, you should complete the following steps:

1. Download the [tweeter-web-starter](./tweeter-web-starter.zip) zip file.
1. Unzip the file into a parent directory of your choice.
1. Open the project in VS Code and follow the instructions in the Readme to compile and run the project.
1. Create a StatusItem component (similar to the UserItem component created in the video) to eliminate duplication in the Feed and Story UI.
1. Create an AuthenticationFields component, as described in the video, to remove the duplicate UI logic for displaying the Alias and Password fields in the Login and Register components.
   - **Note:** When you create the AuthenticationFields component, you will notice a slight difference between register and login. There is an onKeyDown event that calls a different method on login vs, register. You can handle that by passing the function to be called as a prop into the AuthenticationFields component.
1. Create an OAuth component, as described in the video, to remove the OAuth fields from the AuthenticationFormLayout component.

## Part 2: More React Components

In part two of the assignment, you will eliminate additional duplication from the UI by creating an additional React component. This [demo video](https://youtu.be/MeXHcXQpUCY) shows you how to create the component by creating a similar one for you. After watching the video, do the following:

1. Create the UserItemScroller component as shown in the video.
2. Eliminate the duplication in FeedScroller and StoryScroller by creating a StatusItemScroller, similar to the UserItemScroller created in the video and in the previous step.

## Part 3: React Hooks

In part three of the assignment, you will improve the provided Tweeter UI by adding react hooks. This [demo video](https://youtu.be/wEXQCA4Foww) shows you how to create React hooks by creating two hooks for you. After watching the video, you should complete the following steps:

1. Create **BOTH** the useMessageActions and useMessageList hooks as shown in the video.
1. Create **BOTH** the useUserInfoActions and useUserInfo hooks described in the video to eliminate component dependencies on the UserInfoActionsContext and the UserInfoContext.
1. Create the useUserNavigation hook described in the video to eliminate code duplication.

## Submission Instructions

- Pass off your project with a TA by the due date before TA's are off for the day (you must be in the pass off queue 1 hour before they are off to guarantee same-day pass off)
- You can only passoff once
- If you passoff before the passoff day, you will get an additional 4% of extra credit for this assignment

## [Milestone 1 FAQ](./milestone-1-faq.md)
