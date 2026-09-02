# DynamoDB Exercise Part 2

> **AI usage for this assignment is Level 1 — Reference** (see the [AI Usage Level Definitions](https://claude.ai/public/artifacts/b1e35a55-f03f-4120-ba4f-b83a1ee3fc83)), the course default: AI helps you learn, but it never sees your work.
  
In this exercise you will write a program that uses the AWS SDK to:
1. Query the “follows” table with pagination
1. Query the “follows_index” index with pagination

## Assumptions
You have already configured your AWS credentials on your computer.  This should have been done when you installed the AWS CLI on your computer.

You have completed DynamoDB Exercise Part 1

## Instructions

Write code to do paged queries (using the limit parameter) on the “follows” table and the “follows_index” index.

**Note:** There are code examples of most of what you need to do in this exercise in this Github repo: https://github.com/BYU-CS-340/dynamodb-samples-typescript. You can also refer to this example on how to query a table: https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/dynamodb/command/QueryCommand.

## Steps

1. Write code to do a paged query (using the limit parameter) on the "follows" table to return a page of followees of a specified follower. Your method should have the following signature:
    ```
    async getPageOfFollowees(followerHandle: string, pageSize: number, lastFolloweeHandle: string | undefined): Promise<DataPage<Follows>> { ... }
    ```
1. Write a main function that calls your method to get the first and second pages of followees of a specified follower.
1. Write code to do a paged query on the "follows_index" to return a page of followers of a specified followee. Your method should have the following signature:
    ```
    async getPageOfFollowers(followeeHandle: string, pageSize: number, lastFollowerHandle: string | undefined): Promise<DataPage<Follows>> { ... }
    ```
1. Add calls to your main function to get the first and second pages of followers of a specified followee.

## Submission

On Canvas submit your individual .ts files. DO NOT SUBMIT A ZIP FILE.

## More Information

The full AWS SDK documentation can be found here in the SDKs section: https://aws.amazon.com/tools/
