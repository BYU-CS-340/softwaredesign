# Milestone 3 FAQ

This document is organized according to what parts of the project you are working on.

## Table of Contents

- [Milestone 3 FAQ](#milestone-3-faq)
  - [Table of Contents](#table-of-contents)
  - [M3 Client Side Functionality](#m3-client-side-functionality)
    - [Registering a new User: How do I pass the image to the server façade? What data type?](#registering-a-new-user-how-do-i-pass-the-image-to-the-server-façade-what-data-type)
    - [How do I stop the first page loading twice?](#how-do-i-stop-the-first-page-loading-twice)
  - [M3 Requests and Response Objects](#m3-requests-and-response-objects)
    - [Why are all of the fields in my request/response objects null when run on Lambda?](#why-are-all-of-the-fields-in-my-requestresponse-objects-null-when-run-on-lambda)
    - [Some of my fields are null in my request/response objects once they pass API Gateway. I have getters, setters, and a default constructor. What's going on?](#some-of-my-fields-are-null-in-my-requestresponse-objects-once-they-pass-api-gateway-i-have-getters-setters-and-a-default-constructor-whats-going-on)
    - [The fromJson functions are reading values as undefined or null](#the-fromjson-functions-are-reading-values-as-undefined-or-null)
  - [M3 Server-side Functionality](#m3-server-side-functionality)
    - [Do I need to persist any data for Milestone 3?](#do-i-need-to-persist-any-data-for-milestone-3)
    - [What should be in my DAOs and what should be in my Services?](#what-should-be-in-my-daos-and-what-should-be-in-my-services)
    - [Should my services access more than one DAO?](#should-my-services-access-more-than-one-dao)
    - [Error: tweeter-shared not found](#error-tweeter-shared-not-found)
    - [Error: cannot read undefined calling 'split'](#error-cannot-read-undefined-calling-split)
    - [Data sometimes not returned](#data-sometimes-not-returned)
  - [API Gateway](#api-gateway)
    - [Why isn't API Gateway passing my requests correctly to Lambda?](#why-isnt-api-gateway-passing-my-requests-correctly-to-lambda)
    - [How do I debug the Gateway?](#how-do-i-debug-the-gateway)
    - [What kind of documentation do I need to have for each method?](#what-kind-of-documentation-do-i-need-to-have-for-each-method)
    - [What do I do with a 403 Error?](#what-do-i-do-with-a-403-error)
  - [Lambda](#lambda)
    - [Do I have to re-upload my code to each lambda every time I make a change to my server?](#do-i-have-to-re-upload-my-code-to-each-lambda-every-time-i-make-a-change-to-my-server)
<!-- WE NEED A NEW DIAGRAM FOR THIS -->
<!-- - [M3 Sequence Diagram](#sequence-diagram)
    - How do I show X on the diagram? How do I show the Client Communicator calls API Gateway, which then calls AWS Lambda, which then invokes a handler? -->

## M3 Client Side Functionality

### Registering a new User: How do I pass the image to the server façade? What data type?

The starter code for tweeter already transforms the image data into a Base64 encoded string. This can be passed to the server facade and to the backend inside your RegisterRequest. Eventually, in milestone 4, your code will decode the image back into image data to be stored in AWS S3. After storing it in S3, your code will save the URL of the saved image with the new User that is created by registering.

### How do I stop the first page loading twice?

First, make sure that the ClientCommunicator and ServerFacade have .ts extensions and not .tsx extensions.

If that's not the issue, remove strictmode from the index.ts file. Strictmode is using during development and renders the page twice on the first load, first to run a test on the rendering of the page, and then for the actual page. This runs useEffect twice which then loads the first page twice.

See [this discussion](https://stackoverflow.com/questions/60618844/useeffect-is-called-twice-even-if-an-empty-array-is-used-as-an-argument) on StackOverflow for more details. This link also shows some workarounds for keeping strictmode by adding cleanup functions to the useEffect.

## M3 Requests and Response Objects

### Why are all of the fields in my request/response objects null when run on Lambda?

The object will be returned as type json, meaning it will have the same data as the request/response object, but without the functions. If some fields are private, this means that the getter setter functions have also been removed. A fromJson function will need to be added to the request/response object. Look at the entities-Status, User, for example, to see how to create a fromJson function. Or don't make the variables private.

### Some of my fields are null in my request/response objects once they pass API Gateway. I have getters, setters, and a default constructor. What's going on?

By far the largest issue here is that AWS does NOT look at your field names when deciding how to serialize things. It will look at the getters and setters to determine the field names. For example, if you have a field called 'feed', a getter called 'getPosts', and a setter called 'setFeed', AWS will use the getter when serializing and create a field called 'posts' in your JSON object. When deserializing, AWS will then look for a method called 'setPosts', which in this example does not exist, and as a result your 'feed' field will be null. Therefore, it is important that your getters and setters have the same field name in their names.

### The fromJson functions are reading values as undefined or null

Make sure that JSON.stringify is not called on a value that is already a string. If an event comes into the server, the event itself will be of type Object, but some of the subfields, such as event.lastUser, may come in unexpectedly as strings even if typescript demands it be of type User. Calling JSON.stringify on event.lastUser will thus make values unavailable to the fromJson function and is difficult to debug since the data looks like it is there correctly but is doubly stringified. The value can be typecast to string by calling (event.lastIUser as unknown as string).

Even if typescript indicates that event.lastUser is of type User, it is not since custom types can't travel through the internet. Furthermore, since the code is transpiled to javascript before uploading to lambda, it does not do type-checking. To debug this, use 'console.log('typeof event.lastUser: ' + typeof event.lastUser), which will reveal that it is already of type string in spite of what typescript says.

If JSON.stringify is what is needed in the code, do not call JSON.stringify on a value if it is null. JSON.stringify(null) returns the string "null". If the string "null" is passed into a fromJson function this will break the code. A check will have to be made to only call JSON.stringify if the value is not null. This ternary operation will fix the null problem, event.lastItem ? JSON.stringify(event.lastItem) : null.

## M3 Server-side Functionality

### Do I need to persist any data for Milestone 3?

No, persistence would be impossible at this point due to how AWS Lambda works. Since we are not connected to any database, your application should work exactly the same as it did in Milestone 2. Your server should return hardcoded dummy data.

### What should be in my DAOs and what should be in my Services?

For M3, you are not required to code up DAOs. In the sample code, we have included a DAO which returns dummy data to the service layer as an example of M4 functionality and how your service and DAO layers interact. For M3, you can put the dummy code in your service layer. When your app is complete, the service layer will contain all the business logic for your backend and the DAO layer will contain all the logic relating to persisting your app's data.

### Should my services access more than one DAO?

Again, for M3, you are not required to code up DAOs. We've included this FAQ in case you are interested in how, in the future, your service layer will communicate with the DAO layer. In many cases, yes. There are several functions of your application that will need to access more than one table in your database. (For example, logging in will interact with both the User and AuthToken tables.) In such cases, your service will interact with the DAO of each table that it needs to use. (Remember that a DAO represents a single table in your database)

### Error: tweeter-shared not found

There's four things to look out for:

1. The node_modules should be moved into (not renamed to) a nodejs folder, per the lambda exercise.
1. The nodejs folder should be checked to make sure the tweeter-shared folder is not empty, per the m3 spec.
1. The tweeter-shared module's dist folder should immediately (not in a subdirectory) have some index files and a model and util directory, otherwise it's being built wrong.
1. The imports/exports in server and shared need to be correct. The server imports should be from "tweeter-shared" directly, not from a subdirectory. The index.ts should export from ./model/.., not from../src/model/.. Using visual studio code's refactoring utility to move files can change imports in files that were otherwise unedited. Those will need to be located and fixed.

### Error: cannot read undefined calling 'split'

This error typically happens when the lambda layer or the lambda was not uploaded correctly and so the lambda and lambda layer are not matching.

To fully upload the new lambda layer:

1. To be fully confident that the lambda layer is updating correctly, delete the old nodejs.zip, nodejs, and node_modules.
1. Reinstall the modules, then recreate the nodejs and nodejs.zip.
1. Upload the nodejs.zip to a new lambda layer version.
1. Go to each lambda, and update the lambda layer version for them. If using the script, update the lambda layer version in the .server file then run the script.

### Data sometimes not returned

Async/await functions can be tricky. If there is a string of async functions calling async functions, and even just one of those functions is not awaited, the entire functionality can be lost.

Some further tricky things:

The forEach function does not wait for awaited functions, even when await is called. Instead, a for loop will need to be used.

If await is called on a function, but .then is called on the end of the function, then the function will not be awaited. The .then must be removed.

## API Gateway

*See also Requests and Response Objects Section of the FAQ if your issues are about data coming from or going to the client.*

### Why isn't API Gateway passing my requests correctly to Lambda?

Make sure that you have correctly configured the Request Models. You'll need getters and setters for each property. If you are passing data outside of the request body (i.e. in the URL), ensure you have the correct Mapping Templates (see the API Gateway Exercise for how to do this).

### How do I debug the Gateway?

There are several locations to check. First, you can debug the server code line by line using a Main function. Note that if you are debugging the register functionality, you will have to pass in an encoded string for the userImage. Then you can check the AWS lambda test. Then you can check the API Gateway test.

If you run your app, you may want to put a breakpoint on the switch statement for status codes within the ClientCommunicator. Check the responseString here. If the responseString changes during Json deserializing, you may have problems with your request and response constructors.

Finally, you can always check the cloud logs for your lambdas. If these tell you that access has been denied, you may have to update your lambda role permissions.

### What kind of documentation do I need to have for each method?

 Just a short, sweet description of the endpoint so we know what it does when looking at your swagger submission. Here are two examples:

Login: *Logs the user in with the specified username and password*

Register: *Creates a new tweeter user*

### What do I do with a 403 Error?

Typically, 403 is a result of one of two things:

1. The API has not been redeployed since the endpoint you are trying to reach was added.
1. The URL you're trying to use is misspelled.

The first one is easily fixed by redeploying the API. The second can be tested by running the request in an external http service like Postman.

If these don't fix your issue, feel free to speak with a TA.

## Lambda

*See also Requests and Response Objects Section of the FAQ if your issues are about data coming from or going to the client.*

### Do I have to re-upload my code to each lambda every time I make a change to my server?

It does need to happen, but you don't have to do it manually. See [Automating AWS Resource Management](./automating-aws-resource-management.md) for details.
