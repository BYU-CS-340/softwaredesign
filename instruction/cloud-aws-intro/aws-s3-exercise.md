# AWS S3 Exercise
  
In this exercise you will write a program that uses the AWS SDK to upload a local file into an S3 bucket.

## Assumptions

You have already configured your AWS credentials on your computer.  This should have been done when you installed the AWS CLI on your computer.

## Steps

- Create a new typescript project for this exercise
    - **Note:** You can follow [these instructions]([url](https://github.com/BYU-CS-340/softwaredesign/blob/main/instruction/typescript-fundamentals-1/standalone-typescript-project-setup-instructions.md)) to setup a standalone TypeScript project
- Install the AWS S3 SDK Client
    - Run `npm install @aws-sdk/client-s3`
    - Run `npm install @types/aws-sdk --save-dev`
- Create a Typescript source file containing the following code:
    ```
    import fs from "fs";

    import {
        S3Client,
        PutObjectCommand,
    } from "@aws-sdk/client-s3";

    s3upload();

    async function s3upload() {
        if (process.argv.length !== 3) {
            console.log("Specify the file to upload on the command line");
            process.exit();
        }

        try {
            const client = new S3Client();
            const fileContent = fs.readFileSync(process.argv[2]);

            const params = {
                "Body": fileContent,
                "Bucket": //TODO: Specify your bucket name,
                "Key": //TODO: Specify name or file path you want to appear in S3,
            }

            const command = // TODO: Create the PutObjectCommand
            const response = // TODO: Send the command and await the result

            console.log("File upload successful with ", response.$metadata.httpStatusCode);
        } catch (error) {
            console.log(error);
        }
    }
    ```
- Refer to the online documentation for the AWS javascript SDK to learn how to fix the TODOs in the provided code:
    1. https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/
    1. https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/s3/command/PutObjectCommand/
- Test your program by using it to upload some local files into an S3 bucket in your AWS account.
    - Assuming you named your file s3upload.ts and placed it in the src directory, run it with the following command: `npx ts-node src/s3upload.ts <path to file to upload>`
    **Note:** Make sure the bucket name you specify exists in the region configures as your default region. You can find your default region with the following terminal command: `aws configure get region`

## Submission

On Canvas submit the single .ts file containing the code to upload to S3. DO NOT SUBMIT A .zip FILE.

## More Information

The full AWS SDK documentation can be found here in the SDKs section: https://aws.amazon.com/tools/
