# Mapping Exceptions to Error Codes in API Gateway

HTTP status codes indicate to a client whether a request completed successfully or unsuccessfully: `500 Server Error`, `404 Not Found`, etc. (A quick Google will show you the standardized status codes for HTTP.) The backend of your course project should return error codes if it receives invalid requests.

Here's some information about what's happening. Detailed instructions are at the end.

## Throw Runtime Exceptions in Lambda Code

You can trigger API Gateway to send an HTTP error code back to the client by throwing uncaught Error objects in your Lambda code. In each exception message, you should begin the message with an identifier to indicate what type of exception occurred. API Gateway will use this message string to determine what error code to return.

In the example code below, note the messages passed into the RuntimeException objects.

```java
public class UserService extends Service {

    public GetUserResponse getUser(GetUserRequest request) {

        if (!getAuthTokenDAO().isValidAuthToken(request.getCurrentUserAlias(), request.getAuthToken()))
            throw new RuntimeException("[AuthError] unauthenticated request");

        User user = getUserDAO().getUser(request.getAlias());

        if (user == null)
            throw new RuntimeException(String.format("[Bad Request] requested user %s does not exist", request.getAlias()));

        return new GetUserResponse(user);
    }
}
```

## Mapping Exceptions to Error Codes

You can map specific `Error(msg)` messages to response statuses with a **Lambda Error Regex**. API Gateway uses Java-style regular expressions for response mapping (see [Pattern](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)).

In the example below, the regex expression `^\[Bad Request\].*` tells the method to return a 400 status if the string "[Bad Request]" is found in the response at the beginning of a line.  **Note** the space between `Bad` and `Request`, and the (escaped!) square brackets. Make sure it matches the error string you intend to use.

![Integration Response](./integration-response-1.png)

## Setting up HTTP status codes with API Gateway

1. Specify the HTTP status codes for each method.
    1. Select the method's Method Response tab.
    2. Click Create Response.
    3. Enter the status code number. (The default is 200.)
    4. Under Response Body, click Add Model.
    5. For content type, enter `application/json`, and for Model, select Error. (It might look like it's already filled in, but it's not.)
    6. Click Save.
2. Tell API Gateway how to map a thrown error to the Method Response you just created.
    1. Warning: do `500 Server Error` last. This will allow you to have a catch-all at the end.
    2. Select the method's Integration Response tab.
    3. Click Create Response.
    4. Enter the regex you want to use to trigger the HTTP status code. For example, `^\[Bad Request\].*` or `^\Unauthorized: .*`. Keep track of escaped-square-brackets and spaces!
        - It's a good idea to have a catch-all error response, by having `.*` map to a 500 error. API Gateway defaults to `200 OK`, so if you don't, a typo like `throw new Error("Bad Request ...");` (note the missing square brackets) can result in a false success message.
3. Re-deploy your API.
4. In API Gateway, you can test your status code mappings from the method's **Test** page.

## Additional Helps

- [Handle Lambda Errors in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/handle-errors-in-lambda-integration.html)
- [Error Handling Patterns in Amazon API Gateway and AWS Lambda](https://aws.amazon.com/blogs/compute/error-handling-patterns-in-amazon-api-gateway-and-aws-lambda/)
- [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- [Pattern (Java Regex)](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)
