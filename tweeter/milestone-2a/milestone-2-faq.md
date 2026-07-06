# Milestone 2 FAQ

## What functionality should my app have for this milestone?

In terms of functionality your app should respond similarly to the video, with the notable exceptions that:

1. You will always log in to the dummy user regardless of the supplied credentials.
1. If you register a new user you'll still just get the dummy user
1. When you go to another user's page the Follow button is randomized, pressing it will not be saved, nor will it update counts.
1. If you post a status it will not show up in your Story.

## Do I need to persist any data for Milestone 2?

No, your app should just use hardcoded dummy data.

## How many presenters should I have?

In MVP generally there is only one presenter per view. For the course project you should have only one presenter per view (React component or hook). Some components and hooks that deal only with display logic do not need a presenter. In total, you should end with at least 12 presenter classes.

## Can there be more than one presenter per view?

Technically yes, however, in MVP generally there is only one presenter per view. For the course project you should have only one presenter per view.

## Can I combine a lot of functions into services which have more than one function?

You can, however you should be careful to not violate the Single Responsibility Principle.

## Is it critical that we preserve the exact wording of the toast messages?

As long as the toast message correctly conveys what went wrong or right it is sufficient. If your toast just always says "Something went wrong" that wouldn't be helpful

## Is it OK to convert the image to a byte string in the view, or is that something that should be done in the presenter?

Generally anything dealing with logic should be moved to the presenter.

## Where do I handle password validation?

Basic password validation should happen in your presenter class.

## If I don't have the URLs or Mentions clickable, will I lose points?

Yes, it is expected that URLs and Mentions are clickable and take you to a webpage or user page respectively.

## What does the rubric mean by functional?

For Milestone 2, by functional we mean that your application should appear to work and make the appropriate calls to the backend. It should not crash or hang up. For example, when a user posts a status it should appear to work, however, the status does not need to appear in the feed or story.

## Can I ask the TAs to review my UMLs?

You can ask the TAs specific questions about your UMLs. We cannot do a general review of your UMLs.

## How do I show an Asynchronous call in a Sequence Diagram?

This one-minute [video](https://www.youtube.com/shorts/QRA1Xes6VDo) explains how to represent an asynchronous message in a sequence diagram.

## Can I ask the TAs to look over my architecture?

You can ask the TAs specific questions about your architecture. We cannot do a general review of your architecture.

## What causes this error: RangeError: Maximum call stack size exceeded?

1. If you have the view variable stored in the base presenter, and then you typecast the Presenter.view to ChildPresenter.View (e.g., AuthenticatePresenter.View), using `get View(): {return this._view as AuthenticateView; }`, then this error will occur.
   The keyword `super` should be used to fix this. That is, `get View(): {return super.view as AuthenticateView; }`
1. If a private variable is called using a getter instead of the actual variable, this can cause the problem. For example, calling `this.lastItem` instead of `this._lastItem`.

## What causes this error: TypeError: UserInfoHook_1.userUserInfo.mockReturnValue?

The useUserInfo function must be the default export of the UserInfoHook. Notice that the jest.mock function specifies the file name UseInfoHook but doesn't specify the function name useUserInfo. Thus, this jest.mock function expects there to be a default function.

## Why are jest or other imports not recognized in my component test?

1. Make sure the component test has the .tsx extension.
1. Do not place the tests inside of the \_\_mocks\_\_ folder. The underscores signify that it is a configuration directory, and so the files are treated differently and can mess up the tests.
