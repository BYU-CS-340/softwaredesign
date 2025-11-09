# DynamoDB Exercise Part 1
  
In this exercise, you will:

1. Create a DynamoDB table named “follows” that keeps track of which users are following each other (as in Twitter).
1. Create a secondary index named “follows_index” on the “follows” table.
1. Write a program that uses the AWS SDK to insert, update, and delete items in the “follows” table

## Assumptions

You have already configured your AWS credentials on your computer.  This should have been done when you installed the AWS CLI on your computer.

## Steps

IMPORTANT: Update your IAM user to have DynamoDBFullAccess permissions before beginning this exercise.

1. Go to the AWS DynamoDB web console
1. Create a table named “follows”.
    1. Partition Key: a string named “follower_handle”. This attribute stores the Twitter handle of the user who is following another user.
    1. Sort Key: a string named “followee_handle”. This attribute stores the Twitter handle of the user who is being followed.
    1. This Primary Key lets you query the table by “follower_handle” and sort the results by “followee_handle”.
    1. Under 'Table Settings' click 'Customize settings'.
    1. Adjust read & write capacities by turning autoscaling off for each and entering in the desired provisioned capacity units.  These control how fast Dynamo can read/write data to/from your table (you may need to re-adjust these for the project).
    1. Under 'Secondary indexes' click 'create global index'. **Note: The Global Secondary Index will swap the keys so you can also query in the opposite direction (finding who follows @bob, instead of who @bob is following).**
    1. Index Partition Key: a string named “followee_handle. This is the same “followee_handle” attribute you created in the previous step.
    1. Index Sort Key: a string named “follower_handle”. This is the same “follower_handle” attribute you created in the previous step.
    1. This Primary Key lets you query the index by “followee_handle” and sort the results by “follower_handle”.
    1. Set the index name to “follows_index”.
    1. For “Projected attributes”, keep the “All” setting.
    1. Click 'Create table' at the bottom of the page.
1. Next, write a program that performs operations on the “follows” table.
1. Create a directory for your project and initialize it with `npm init --y`
1. Install the following dependencies using npm install:
    1. ts-node
    1. @aws-sdk/lib-dynamodb
1. Write a Typescript DAO that can put, get, update, and delete items in the “follows” table. You can use the sample code for the "visitor" example covered in class as an example: https://github.com/BYU-CS-340/dynamodb-samples-typescript.git. Alternatively, the links on the following page demonstrate how to put, get, update, and delete items in a DynamoDB table: https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/dynamodb/.  The complete online documentation for the AWS JavaScript SDK can be found here: https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/. 
1. Write a program that uses your DAO to do the following:
    1. “Put” 25 items into the “follows” table all with the same follower:
        1. “follower_handle”: string handle of the user who is following (e.g., “@FredFlintstone”)
            1. This attribute should be the same across all of these 25 items
        1. “follower_name”: string name of the user who is following (e.g., “Fred Flintstone”)
            1. This attribute should be the same across all of these 25 items
        1. “followee_handle”: string handle of the user being followed (e.g., “@ClintEastwood”)
            1. This attribute should be different for each of these 25 items
        1. “followee_name”: string name of the user being followed (e.g., “Clint Eastwood”)
            1. This attribute should be different for each of these 25 items
    1. "Put" 25 more items into the "follows" table, this time all with the same followee:
        1. “follower_handle”: string handle of the user who is following (e.g., “@FredFlintstone”)
            1. This attribute should be different for each of these 25 items
        1. “follower_name”: string name of the user who is following (e.g., “Fred Flintstone”)
            1. This attribute should be different for each of these 25 items
        1. “followee_handle”: string handle of the user being followed (e.g., “@ClintEastwood”)
            1. This attribute should be the same across all of these 25 items
        1. “followee_name”: string name of the user being followed (e.g., “Clint Eastwood”)
            1. This attribute should be the same across all of these 25 items
    1. “Get” one of the items from the “follows” table using its primary key
    1. “Update” the “follower_name” and “followee_name” attributes of one of the items in the “follows” table
    1. “Delete” one of the items in the “follows” table using its primary key

## Submission

On Canvas submit your individual .ts files. DO NOT SUBMIT A ZIP FILE.
 

## More Information

The full AWS SDK documentation can be found here in the SDKs section:https://aws.amazon.com/tools/
