# Bugs Hate These 5 Network Debugging Tips (#2 will surprise you)

Debugging errors between the client and server can sometimes be difficult without the right tools or information. These tips will provide some starting points for finding the source of any errors related to API calls and your server-side code that's hosted on AWS.

## Tip 1: Run the code on your own machine

Although your server code will never be run on your own machine in production, you can still manually run it on your own machine, which will give you access to whatever tools are available in your IDE, notably the debugger. Your code can be run in one of two ways:

1. Create unit tests. You will already be creating several tests for Milestone 3. However, these tests only directly run client-side code. It may benefit you to write additional unit tests in your tweeter-server directory that can directly call any back-end handlers or services you wish to test. The tests do not have to be robust. Their biggest benefit will be providing a way to directly run your server-side code.
1. Run a file that calls the functions you wish to test. Typescript/javascript projects don't need main files; code in files can be directly run from top to bottom. You can create a new file, put any code you wish to run in it, and then run it using `npx ts-node src/my-file.ts` (this will transpile and run the code). For example, to run your LoginHandler, your file can be as simple as this:

```typescript
import { handler } from "./lambda/LoginHandler";

handler(new LoginRequest("alias", "password"));
```

## Tip 2: Use the devtools window on your browser

When you get any strange behavior while running your website, or if you get any error toast messages in the top right corner that aren't very clear, always check your devtools window. It can be opened by right-clicking on the webpage and selecting "Inspect". This will take you to the Elements tab by default. You can also directly open the console tab with the shortcut `Ctrl+Shift+J` (Windows, Linux) or `Command+Option+J` (macOS).

There are three tabs that will benefit you the most:

1. The Console tab will display any messages that have been logged to the console by your code, as well as most exceptions (shown in red). The error messages often provide a helpful stack trace, and you can track all of the console.log() statements you may have written.
1. The Network tab shows all network requests that have been made by the current page. Sometimes it doesn't begin to track requests until it is opened. When a request is clicked, you will see another list of tabs that display helpful information. The Headers tab shows all outgoing information that was sent, while the Preview or Response tabs shows the response that was returned by the API. Whenever I run into an issue (especially dealing with serialization), I always check the Preview tab on the request and see if a proper response was returned. If so, it narrows down the issue to the front-end code; otherwise, you know the issue is on the back-end.
1. The Sources tab can be used to set breakpoints and step through your transpiled front-end code. You should see the file structure on the left. You can also hit Ctrl/Command+P to open a file by name. Breakpoints can be set by clicking on the line numbers.

## Tip 3: Test on the Lambda console

The Test tab in the lambda console will allow you to test a lambda function directly. Go to the lambda function you wish to test, click on the Test tab, and input the correct json to match the type of request that your function expects. For example, the LoginLambda's json would require something like this:

```json
{
  "alias": "alias",
  "password": "password"
}
```

For some functions, the request json may look a bit more complicated. It may be helpful to directly copy the proper request json from your website's requests. This can be done with the following steps:

1. go to the devtools Network tab mentioned above
1. perform the actions necessary to make the request (e.g. logging in, scrolling down to load a second page of statuses, etc.)
1. select the network request you are trying to test
1. click on the Headers tab
1. highlight and copy the json under the "Request Payload" section

After filling out the json, you may need to name and save the test event. When you are ready, hit "Test" at the top and it will trigger the lambda function with the provided json. When it is finished, it will give you a dropdown with a "test succeeded" or "test failed" message. Clicking on the dropdown will give you any log statements or the stack trace for the error.

It is also helpful to look at the Cloudwatch logs for the given lambda. Under the Monitor tab, select "View Logs in Cloudwatch." This will take you to a logs page where you can select recent invocations of the lambda function and view any console.log() statements, execution times, etc.

## Tip 4: Test on API Gateway

If you think the error is occurring with the actual http request or response, you can use the test function in the API Gateway console. In the console, navigate to the POST (or other method) resource that you wish to test. Click on the Test tab. This will bring up a fillable form to create a request to test the resource with. As mentioned in Tip 3, you can use the Network/Headers tab in the devtools window to copy all the proper information for the request.

## Tip 5: Test using curl or Postman

It may also be helpful to directly send an API request using curl or Postman instead of through the Tweeter website. You can easily copy a curl request from the Network tab in the devtools window. Just right-click on the request and select "Copy", then "Copy as cURL". You can then paste it into your curl request or import it to your postman request and use it as you see fit.

For those of you unfamiliar with Postman, it provides a simple UI to send API requests with. You can go to postman.co, create a free account, create a workspace, and then start using your api request by pressing Import and then pasting your curl request that you copied. You can also set up the request url, headers, and body manually.
