# Project Milestone 2 Part C: Automated Testing

For this milestone you will write tests to test some of the presenters and some of the components of your Tweeter application. In practice, you should write many more tests than we are requiring in this assignment.

## Testing Configuration

You will need to install multiple testing packages and do some configuration before you can write the tests for this assignment. Follow [this tutorial](./tweeter-project-testing-setup-tutorial.md) to do the required testing setup.

## Presenter Tests

Use Jest and ts-mockito to write tests for two of your presenters: AppNavbarPresenter and PostStatusPresenter. We show you step-by-step how to test AppNavbarPresenter in [this video](https://youtu.be/sZ2ezpJXXQo).

### AppNavbarPresenter Test

After (or while) watching the video, write AppNavbarPresenter tests as shown in the video. When you are through, your tests should test the following functionality of the presenter's logout method:

1. The presenter tells the view to display a logging out message.
1. The presenter calls logout on the user service with the correct auth token.
1. When the logout is successful, the presenter tells the view to clear the info message that was displayed previously, clear the user info, and navigate to the login page.
1. When the logout is not successful, the presenter tells the view to display an error message and does not tell it to clear the info message, clear the user info or navigate to the login page.

### PostStatusPresenter Test

Write PostStatusPresenter tests similar to the AppNavbarPresenter tests demonstrated in the video. Specifically, your tests should test the following functionality of the postStatus method:

1. The presenter tells the view to display a posting status message.
1. The presenter calls postStatus on the post status service with the correct status string and auth token.
1. When posting of the status is successful, the presenter tells the view to clear the info message that was displayed previously, clear the post, and display a status posted message.
1. When posting of the status is not successful, the presenter tells the view to clear the info message and display an error message but does not tell it to clear the post or display a status posted message.

## Component Tests

Use Jest, ts-mockito and React Testing Library to write tests for two of your Components: Login and PostStatus. We show you step-by-step how to test Login in [this video](https://youtu.be/ZDRS5fqIemU).

### Login Component Test

After (or while) watching the video, write Login component tests as shown in the video. When you are through, your tests should test the following functionality of the Login component:

1. When first rendered the sign-in button is disabled.
1. The sign-in button is enabled when both the alias and password fields have text.
1. The sign-in button is disabled if either the alias or password field is cleared.
1. The presenter's login method is called with correct parameters when the sign-in button is pressed.

### PostStatus Component Test

Write PostStatus component tests similar to the Login component tests demonstrated in the video. Specifically, your tests should test the following functionality of the PostStatus component:

1. When first rendered the Post Status and Clear buttons are both disabled.
1. Both buttons are enabled when the text field has text.
1. Both buttons are disabled when the text field is cleared.
1. The presenter's postStatus method is called with correct parameters when the Post Status button is pressed.

**Note:** The PostStatus component uses the userInfoHook and will expect it to return an object containing a current user and an auth token. This will return null when the component is rendered in your test, which will cause the component not to work correctly. To resolve this, you will need to mock the userInfoHook. We've been using ts-mockito for mocking in our tests and Jest as the test runner. However, the easiest way to mock a hook used in a component you are testing, is to use Jest. Here are two simple steps for mocking a hook with Jest:

Above your describe function, use Jest to create a mock of the hook like this:

```typescript
jest.mock("../../src/components/userInfo/UserInfoHooks", () => ({
  ...jest.requireActual("../../src/components/userInfo/UserInfoHooks"),
  __esModule: true,
  useUserInfo: jest.fn(),
}));
```

In a beforeAll function of your test, specify the values the hook returns like this:

```typescript
(useUserInfo as jest.Mock).mockReturnValue({
  currentUser: mockUserInstance,
  authToken: mockAuthTokenInstance,
});
```

## Milestone Report

Create and submit the document described in [Milestone 2 Part C: Documents](./milestone-2c-docs.md).

## Passoff

- **Pass off your project with a TA by the due date at the end of TA hours (you must be in the pass off queue 1 hour before the end of TA hours to guarantee pass off)**
- You can only passoff once.
- If you passoff before the passoff day, you will get an additional 4% of extra credit in this assignment

## Rubric

- (10) Automated Tests
  - (6) Specified tests implemented and working
  - (4) Proper use of Jest, ts-mockito and React Testing Library

Up to (10) points may be docked for incorrect functionality of the application

## [Milestone 2 FAQ](../milestone-2a/milestone-2-faq.md)
