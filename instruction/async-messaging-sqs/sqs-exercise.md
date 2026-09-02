# SQS Exercise

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.

In this exercise you will:

- Create an SQS queue
- Write a program that sends messages to your queue
- Write a Lambda function that processes messages sent to your queue

## Assumptions

You have already configured your AWS credentials on your computer.  This should have been done when you installed the AWS CLI on your computer.

## Steps
- Step 0 - Setup:
    1. Go to the IAM console.
    1. Add the SQS policy to your User.
        1. Find your User.
        1. Select "Add Permissions" then "Attach Policies Directly".
        1. Search for "SQS".
        1. Select "AmazonSQSFullAccess".
        1. Select "Next" and "Attach Permissions".
    1. Add the SQS policy to your Lambda Role.
        1. Find your Role.
        1. Select "Add Permissions".
        1. Search for "SQS".
        1. Select "AmazonSQSFullAccess".
        1. Select "Add Permissions".
    1. If you have both of these ready, you are good to go.
- Step 1 - Queue Creation:
    1. Go the the AWS Simple Queue Service (SQS) web console
    2. Create a new SQS queue (*Note: The SQS queue you created when doing the pre-class preparation was a FIFO queue. For this exercise we are using a Standard queue. Since you cannot change the type of a queue after it is created, you need to create a new queue for this exercise.*)
        1. Click the “Create Queue” button
        1. Select “Standard” type (not "FIFO")
        1. Enter a name for your queue
        1. Create with default settings (you can also specify other accounts, IAM users, and roles are allowed to interact with the queue)
- Step 2 - Send a message to your queue through the SQS web console:
    1. Click on your new queue
    1. On the queue page, click the "Send and receive messages" button
    1. Send a message 
        1. Provide some text for the message body
        1. Click the “Send Message” button
    1. Verify that the message was sent
        1. Click “Poll for Messages”
        1. You should see the message you sent in the queue (click More Details to see more details)
- Step 3 - Write a program that sends messages to your queue:
    1. Create a new Typescript project. Refer to the [Typescript Projects Slides](https://docs.google.com/presentation/d/1px37Y7yS8KsMWoY6GpkyO5h4tu5TNXtpMNfK8GQx6eI/edit?usp=sharing) or the [Image Editor](../../image-editor/image-editor.md) assignment if you need a refresher on how to do this.
    1. Create a file named SqsClient.ts containing the following code.  The queue URL can be found in the SQS console.
        ```
        import { SQSClient, SendMessageCommand } from "@aws-sdk/client-sqs";

        let sqsClient = new SQSClient();

        async function sendMessage(): Promise {
        const sqs_url = "*** PUT YOUR QUEUE URL HERE ***";
        const messageBody = "*** PUT YOUR MESSAGE BODY HERE ***";

        const params = {
            DelaySeconds: 10,
            MessageBody: messageBody,
            QueueUrl: sqs_url,
        };

        try {
            const data = await sqsClient.send(new SendMessageCommand(params));
            console.log("Success, message sent. MessageID:", data.MessageId);
        } catch (err) {
            throw err;
        }
        }

        sendMessage();
        ```
        - More information on accessing SQS from Javascript can be found here:
            - https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/javascript_sqs_code_examples.html.
    1. Install the AWS SDK in your project:
        ```
        npm install @aws-sdk/client-sqs
        ```
    1. Install the Typescript types in your project:
        ```
        npm install --save-dev @types/aws-sdk
        ```
    1. Change your start script in package.json to
        ```
        "start": "node dist/SqsClient.js"
        ```
    1. Test your program
        1. Run your program
        1. Go to your SQS console and verify that the messages sent by your program are showing up in the queue.
- Step 4 - Write a Lambda function that processes messages sent to your queue:
    1. Write the code for your lambda function.
        1. Create a file named QueueProcessor.ts that will contain the “handler” for your Lambda function. Add the following code to this file:
            ```
            export const handler = async function (event: any) {
            for (let i = 0; i < event.Records.length; ++i) {
                const { body } = event.Records[i];
                // TODO: Add code to print message body to the log.
            }
            return null;
            };
            ```
        1. Fill in the code that logs the message body.
        1. **Note:** You can log to an Amazon Cloudwatch log simply by doing a console.log(...)
        1. **Note:** SQS will send more than one message to the same lambda if it has many messages that need to be processed. The for loop in the above code addresses this.
    1. Your Lambda function needs to run with an IAM role that has SQS permissions.  Modify the IAM role you created in the “Lambda / IAM” exercise to allow SQS access:
        1. Go to the IAM console
        1. On the left, select “Roles”
        1. Click on the role you created previously for the “Lambda / IAM” exercise (e.g., cs340lambda)
        1. Click the “Attach Policies” button
        1. On the "Attach Permissions" screen, attach this policy to your role: AmazonSQSFullAccess.  This will allow your Lambda function to access your SQS queues.
- Step 5 - Deploy your Lambda function:
    1. Create the function
        1. Go to your AWS Lambda console
        1. Select “Functions” on the left side of the Lambda screen
        1. Click “Create Function” button
        1. Give your function a name (e.g., queue_processor)
        1. Select “Node.js 20.x” as your function's runtime
        1. Under Permissions, expand "Change default execution role"
        1. Select “Use an existing role” and select the IAM role you configured earlier (e.g., cs340lambda)
        1. Click the “Create function” button. 
    1. Upload your code to the function
        1. Either:
            - Transpile the code, zip it,  and upload to the lambda, or
            - Copy the code text into the lambda in the console, replacing 'event: any' with simply 'event' to make it javascript compatible.
        1. In the Runtime Settings section, make sure that the Handler field points to the handler function: (e.g., QueueProcessor.handler, or index.handler).
- Step 6 - Connect your Lambda function to your SQS queue:
    1. Add a lambda trigger to your queue
        1. Go to your queue on the SQS web console
        1. Select the "Lambda triggers" tab and click the “Configure Lambda Function trigger” button
        1. Select the name of the Lambda function you previously created (e.g., queue_processor)
        1. Click the “Save” button
    1. Now, whenever a message is sent to your SQS queue, the message will be sent to your Lambda function for processing. Verify that this is happening by running your program that sends messages to your queue, or send messages manually through the SQS console.
    1. Test the complete queue
        1. Run your SqsClient again and confirm that the message body appears in the Cloudwatch log for your Lambda function.
        1. **Note:** You can find the CloudWatch log by selecting the 'CloudWatch' service from the AWS console, selecting 'Logs -> Log Groups' and then selecting the log group for your Lambda function.
        1. Click a log stream from within the log group to find the log message containing your message body. 
- Step 7 - Disconnect your Lambda trigger from your SQS queue:
    - **IMPORTANT: After completing the exercise, remove the lambda trigger from your queue, because SQS constantly polls the queue to detect new messages so it can call the lambda appropriately.  All of this polling adds to your AWS charges, so you will want to remove the lambda trigger to avoid this.**
 
## Submission

Submit a screenshot of the AWS Lambda Logs where it prints out the records that are read from the queue.

## More Information

The full AWS SDK documentation can be found here: https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/
