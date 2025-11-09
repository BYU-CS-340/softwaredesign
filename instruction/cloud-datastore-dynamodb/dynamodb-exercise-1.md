# DynamoDB Exercise Part 1
  
In this exercise, you will:

1. Create a DynamoDB table named "follows" that keeps track of which users are following each other (as in Twitter).
1. Create a secondary index named "follows_index" on the "follows" table.
1. Write a program that uses the AWS SDK to insert, update, and delete items in the "follows" table

## Assumptions

You have already configured your AWS credentials on your computer.  This should have been done when you installed the AWS CLI on your computer.

## Steps

IMPORTANT: Update your IAM user to have DynamoDBFullAccess permissions before beginning this exercise.

**Step 1. Go to the AWS DynamoDB web console.**

**Step 2. Create a table named `follows`.**
1. Partition Key: a string named `follower_handle`. This attribute stores the Twitter handle of the user who is following another user.
1. Sort Key: a string named `followee_handle`. This attribute stores the Twitter handle of the user who is being followed.

This Primary Key lets you query the table by "follower_handle" and sort the results by "followee_handle". This is like getting user @bob (partition key) and seeing a sorted result of users that @bob follows (sort key).

3. Under "Table Settings" click `Customize settings`.
1. Adjust read & write capacities by turning autoscaling off for each and entering in the desired provisioned capacity units.  These control how fast Dynamo can read/write data to/from your table (you may need to re-adjust these for the project).
1. Under "Secondary indexes" click `Create global index`. 

This Primary Key lets you query the index by "followee_handle" and sort the results by "follower_handle". The Global Secondary Index will swap the keys so you can also query in the opposite direction (finding who follows @bob, instead of who @bob is following).
    
6. Index Partition Key: a string named "followee_handle". This is the same attribute you created earlier.
1. Index Sort Key: a string named "follower_handle". This is the same attribute you created earlier.
1. Set the index name to `follows_index`.
1. For "Projected attributes", keep the `All` setting.
1. Click `Create index`.
1. Click `Create table` at the bottom of the page.

**Step 3. Initialize a program that will be used for performing operations on the `follows` table.**

1. Create a directory for your project and initialize it with `npm init --y`
2. Install the following dependencies using npm install:
    - `ts-node`
    - `@aws-sdk/lib-dynamodb`

**Step 4. Write a Typescript DAO to put, get, update, and delete items in the `follows` table**

You may use these resources for reference:
- [Visitor Sample Code](https://github.com/BYU-CS-340/dynamodb-samples-typescript.git) from class
- [DynamoDB documentation](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/dynamodb/) for how to put, get, update, and delete items
- [AWS JavaScript SKD documentation](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/)


<br>

1. Insert 25 items with the same **follower**

| Attribute | Description | Example | Same/Different across all 25 items | 
| - | - | - | - |
| follower_handle | Handle of the user who is following | @FredFlintstone | Same |
| follower_name | Name of the user who is following | Fred Flintstone | Same |
| followee_handle | Handle of the user being followed | @ClintEastwood | Different |
| followee_name | Name of the user being followed | Clint Eastwood | Different |

<br>

2. Insert 25 items with the same **followee**


| Attribute | Description | Example | Same/Different across all 25 items | 
| - | - | - | - |
| follower_handle | Handle of the user who is following | @FredFlintstone | Different |
| follower_name | Name of the user who is following | Fred Flintstone | Different |
| followee_handle | Handle of the user being followed | @ClintEastwood | Same |
| followee_name | Name of the user being followed | Clint Eastwood | Same |

**Step 5. Test the DAO**
1. `get` one item using its primary key
1. `update` the `follower_name` and `followee_name` attributes of one item
1. `delete` one item using its primary key

## Submission

On Canvas submit your individual `.ts` files. DO NOT SUBMIT A ZIP FILE.
 

## More Information

The full AWS SDK documentation can be found here in the SDKs section: https://aws.amazon.com/tools/
