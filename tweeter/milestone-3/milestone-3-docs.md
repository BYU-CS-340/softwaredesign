# Project Milestone 3: Documents

As part of Milestone 3 you were required to create your Tweeter Web API in AWS API Gateway and provide documentation for each Web API endpoint. For this assignment you will submit your API documentation. As a reminder, here is the relevant part of the Milestone 3 specification:

## Your Web API will need to provide capabilities such as

1. Sign up a user
1. vSign in a user
1. Follow a user
1. Determine if a user is currently following another user
1. Get a user's followers (must be paginated)
1. Send a status
1. Etc.

This list includes about half of the necessary endpoints. You will need to figure out the other half on your own. When defining your API in the API Gateway, please do the following:

- Provide a description for each API method that you define. To do this:
  - For each method:
    - In the Resources tab, select the method.
    - In the upper right-hand corner of the console, click the grey book icon.
    - Edit the description
    - Click "Save", then "Close"
  - After your API is completed and you have added a description for each method, go to the Documentation tab on the left-hand panel.
  - All of your descriptions should be displayed.
  - In the upper-right-hand corner, click "Publish Documentation"
  - Select the Stage of the API that you will turn in.
  - Input any version number for your documentation.
  - Click "Publish"
  - Now, when you export your Swagger file for the API, it will include your descriptions for each method.
- For each method, ensure you have integration responses for all relevant HTTP status codes (200, 400, 500). Here is an explanation about how to set up integration responses.
  Ensure the code returns some 400 [Bad Request] responses (for example, by throwing an error if the login username is empty). Do not worry about 500 [Internal Server] responses in the code.

## Submission

- Submit to Canvas a zip file containing your project in its current form. If your project is too big to upload to Canvas, you are probably including files that shouldn't be there. Only include files that you would version.
- Submit to Canvas an exported YAML or JSON swagger file that describes your API. To do this export, click on your stage, then the export tab. You will see the option to "export as swagger" (and select "Export with API Gateway extensions").
- Submit to Canvas a PDF file containing a UML sequence diagram demonstrating what happens when a user successfully sends a status, including both client and server side objects.
  - Keep in mind that the asynchronous nature shown in the 2C diagram will still be present.
  - Don't forget to include how the API Gateway bridges the gap between your Client and Server objects
  - This diagram should start with the submitPost() call on your PostStatus component, and end with a displayInfoMessage() call on the component's listener.

## Rubric

- [5 points] For your JSON/YAML swagger file describing your API. For full points your API must meet the requirements described above in the "Web API Endpoint Specifications" section.
- [5 points] For your UML sequence diagram

## [Milestone 3 FAQ](./milestone-3-faq.md)
