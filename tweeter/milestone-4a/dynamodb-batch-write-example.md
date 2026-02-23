# DynamoDB Batch Write Example

In order to meet the [performance requirements for Milestone 4](../milestone-4b/milestone-4b.md), you will need to demonstrate that you can write a status to the feeds of 10,000 followers of a single user. To accomplish this, you will use the Batch-Write functionality of DynamoDB.

To help you understand how to use the Batch-Write properly and without errors, this example will show you how to use it to write 10,000 users to your User Table, and add the "following" relationship between each of them and your test user, using batch-write.

## First, a little about Write Capacity Units

Before you begin:

- Go to the DynamoDB AWS Console
- Make sure the left side-bar is expanded
- Click 'Tables'.
- Select your User table.
- Click on the 'Capacity' tab.
- Scroll down to 'Provisioned Capacity' and look at the field labeled 'Write capacity units'.

In order for this to go at a reasonable speed, the value in this field must be set to a sufficiently high number. For pass-off, you will not be allowed to make this number higher than specified by the [Milestone 4B specification](../milestone-4b/milestone-4b.md). For the purposes of writing the 10,000 user to your database, however, you can crank it as high as you like. Bear in mind that making it too high has the possibility of running you some charges with AWS. Before you run your script, adjust the value in this field to a sufficiently large number (say, 200), and do the same for your Follow table. Make sure to increase the WCU for the follow index as well.

**IMPORTANT: Be sure to lower this number back to 5 after you add all the items to the table or AWS will charge you for the provisioned capacity.**

## Generating 10,000 users

To write 10,000 users and follows to the database, you need to generate data for the users. Here is an example of a script to generate the data.

Before running the script, **add a user with alias '@daisy' to be the followee, and make sure @daisy has 'follower_count' and 'followee_count' attributes with values of 0**. The 'follower_count' attribute is referenced by the script. You will receive an error when the script tries to update the count if the attribute does not exist for the followee (@daisy). You could add the user by making an entry directly in the User table, but you will likely want to login as this user for testing too. To ensure that the @daisy user is properly created, we recommend creating it by registering the user through your tweeter client app.

```typescript
import { FillFollowTableDao } from "./FillFollowTableDao";
import { FillUserTableDao } from "./FillUserTableDao";
import { User } from "tweeter-shared";

// Increase the write capacities for the follow table, follow index, and user table, AND REMEMBER TO DECREASE THEM after running this script

const mainUsername = "@daisy";
const baseFollowerAlias = "@donald";
const followerPassword = "password";
const followerImageUrl =
  "https://faculty.cs.byu.edu/~jwilkerson/cs340/tweeter/images/donald_duck.png";
const baseFollowerFirstName = "Donald";
const baseFollowerLastName = "Duck";

const numbUsersToCreate = 10000;
const numbFollowsToCreate = numbUsersToCreate;
const batchSize = 25;
const aliasList: string[] = Array.from(
  { length: numbUsersToCreate },
  (_, i) => baseFollowerAlias + (i + 1),
);

const fillUserTableDao = new FillUserTableDao();
const fillFollowTableDao = new FillFollowTableDao();

main();

async function main() {
  console.log("Creating users");
  await createUsers(0);

  console.log("Creating follows");
  await createFollows(0);

  console.log("Increasing the followee's followers count");
  await fillUserTableDao.increaseFollowersCount(
    mainUsername,
    numbUsersToCreate,
  );

  console.log("Done!");
}

async function createUsers(createdUserCount: number) {
  const userList = createUserList(createdUserCount);
  await fillUserTableDao.createUsers(userList, followerPassword);

  createdUserCount += batchSize;

  if (createdUserCount % 1000 == 0) {
    console.log(`Created ${createdUserCount} users`);
  }

  if (createdUserCount < numbUsersToCreate) {
    await createUsers(createdUserCount);
  }
}

function createUserList(createdUserCount: number) {
  const users: User[] = [];

  // Ensure that we start at alias 1 rather than alias 0.
  const start = createdUserCount + 1;
  const limit = start + batchSize;

  for (let i = start; i < limit; ++i) {
    let user = new User(
      `${baseFollowerFirstName}_${i}`,
      `${baseFollowerLastName}_${i}`,
      `${baseFollowerAlias}${i}`,
      followerImageUrl,
    );

    users.push(user);
  }

  return users;
}

async function createFollows(createdFollowsCount: number) {
  const followList = aliasList.slice(
    createdFollowsCount,
    createdFollowsCount + batchSize,
  );

  await fillFollowTableDao.createFollows(mainUsername, followList);

  createdFollowsCount += batchSize;

  if (createdFollowsCount % 1000 == 0) {
    console.log(`Created ${createdFollowsCount} follows`);
  }

  if (createdFollowsCount < numbFollowsToCreate) {
    await createFollows(createdFollowsCount);
  }
}
```

Note that there is no code specific to DynamoDB in this script. That is in the DAOs described below.

## DynamoDB Batch Write

The above script uses a FillUserTableDao to batch write users to the database and a FillFollowTableDao to batch write follow relationships. Here is the FillUserTableDao:

```typescript
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import {
  DynamoDBDocumentClient,
  BatchWriteCommand,
  BatchWriteCommandInput,
  BatchWriteCommandOutput,
  UpdateCommand,
} from "@aws-sdk/lib-dynamodb";
import * as bcrypt from "bcryptjs";
import { User } from "tweeter-shared";

export class FillUserTableDao {
  //
  // Modify these values as needed to match your user table.
  //
  private readonly tableName = "tweeter_user";
  private readonly userAliasAttribute = "alias";
  private readonly userFirstNameAttribute = "first_name";
  private readonly userLastNameAttribute = "last_name";
  private readonly userImageUrlAttribute = "image_url";
  private readonly passwordHashAttribute = "password_hash";
  private readonly followeeCountAttribute = "followee_count";
  private readonly followerCountAttribute = "follower_count";

  private readonly client = DynamoDBDocumentClient.from(new DynamoDBClient());

  async createUsers(userList: User[], password: string) {
    if (userList.length == 0) {
      console.log("zero followers to batch write");
      return;
    }

    const hashedPassword = await bcrypt.hash(password, 10);

    const params = {
      RequestItems: {
        [this.tableName]: this.createPutUserRequestItems(
          userList,
          hashedPassword,
        ),
      },
    };

    try {
      const resp = await this.client.send(new BatchWriteCommand(params));
      await this.putUnprocessedItems(resp, params);
    } catch (err) {
      throw new Error(
        `Error while batch writing users with params: ${params}: \n${err}`,
      );
    }
  }

  private createPutUserRequestItems(userList: User[], hashedPassword: string) {
    return userList.map((user) =>
      this.createPutUserRequest(user, hashedPassword),
    );
  }

  private createPutUserRequest(user: User, hashedPassword: string) {
    const item = {
      [this.userAliasAttribute]: user.alias,
      [this.userFirstNameAttribute]: user.firstName,
      [this.userLastNameAttribute]: user.lastName,
      [this.passwordHashAttribute]: hashedPassword,
      [this.userImageUrlAttribute]: user.imageUrl,
      [this.followerCountAttribute]: 0,
      [this.followeeCountAttribute]: 1,
    };

    return {
      PutRequest: {
        Item: item,
      },
    };
  }

  private async putUnprocessedItems(
    resp: BatchWriteCommandOutput,
    params: BatchWriteCommandInput,
  ) {
    let delay = 10;
    let attempts = 0;

    while (
      resp.UnprocessedItems !== undefined &&
      Object.keys(resp.UnprocessedItems).length > 0
    ) {
      attempts++;

      if (attempts > 1) {
        // Pause before the next attempt
        await new Promise((resolve) => setTimeout(resolve, delay));

        // Increase pause time for next attempt
        if (delay < 1000) {
          delay += 100;
        }
      }

      console.log(
        `Attempt ${attempts}. Processing ${
          Object.keys(resp.UnprocessedItems).length
        } unprocessed users.`,
      );

      params.RequestItems = resp.UnprocessedItems;
      resp = await this.client.send(new BatchWriteCommand(params));
    }
  }

  async increaseFollowersCount(alias: string, count: number): Promise {
    const params = {
      TableName: this.tableName,
      Key: { [this.userAliasAttribute]: alias },
      ExpressionAttributeValues: { ":inc": count },
      UpdateExpression:
        "SET " +
        this.followerCountAttribute +
        " = " +
        this.followerCountAttribute +
        " + :inc",
    };

    try {
      await this.client.send(new UpdateCommand(params));
      return true;
    } catch (err) {
      console.error("Error while updating followers count:", err);
      return false;
    }
  }
}
```

Here is the FillFollowTableDao:

```typescript
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import {
  BatchWriteCommand,
  BatchWriteCommandInput,
  BatchWriteCommandOutput,
  DynamoDBDocumentClient,
} from "@aws-sdk/lib-dynamodb";

export class FillFollowTableDao {
  //
  // Modify these values as needed to match your follow table.
  //
  private readonly tableName = "tweeter_follow";
  private readonly followerAliasAttribute = "follower_alias";
  private readonly followeeAliasAttribute = "followee_alias";

  private readonly client = DynamoDBDocumentClient.from(new DynamoDBClient());

  async createFollows(followeeAlias: string, followerAliasList: string[]) {
    if (followerAliasList.length == 0) {
      console.log("Zero followers to batch write");
      return;
    } else {
      const params = {
        RequestItems: {
          [this.tableName]: this.createPutFollowRequestItems(
            followeeAlias,
            followerAliasList,
          ),
        },
      };

      try {
        const response = await this.client.send(new BatchWriteCommand(params));
        await this.putUnprocessedItems(response, params);
      } catch (err) {
        throw new Error(
          `Error while batch writing follows with params: ${params} \n${err}`,
        );
      }
    }
  }

  private createPutFollowRequestItems(
    followeeAlias: string,
    followerAliasList: string[],
  ) {
    return followerAliasList.map((followerAlias) =>
      this.createPutFollowRequest(followerAlias, followeeAlias),
    );
  }

  private createPutFollowRequest(followerAlias: string, followeeAlias: string) {
    const item = {
      [this.followerAliasAttribute]: followerAlias,
      [this.followeeAliasAttribute]: followeeAlias,
    };

    return {
      PutRequest: {
        Item: item,
      },
    };
  }

  private async putUnprocessedItems(
    resp: BatchWriteCommandOutput,
    params: BatchWriteCommandInput,
  ) {
    let delay = 10;
    let attempts = 0;

    while (
      resp.UnprocessedItems !== undefined &&
      Object.keys(resp.UnprocessedItems).length > 0
    ) {
      attempts++;

      if (attempts > 1) {
        // Pause before the next attempt
        await new Promise((resolve) => setTimeout(resolve, delay));

        // Increase pause time for next attempt
        if (delay < 1000) {
          delay += 100;
        }
      }

      console.log(
        `Attempt ${attempts}. Processing ${
          Object.keys(resp.UnprocessedItems).length
        } unprocessed follow items.`,
      );

      params.RequestItems = resp.UnprocessedItems;
      resp = await this.client.send(new BatchWriteCommand(params));
    }
  }
}
```

## How to use this code

To use this code, add the code to tweeter-server. The script (the first block of code) can be placed in any typescript file (i.e BatchLoadScript.ts will work). The other two code blocks are classes that should be placed in .ts files matching the class names. The code will probably need to be placed somewhere under the 'src' folder of tweeter-server. Once you copy the code and make any necessary updates to the table and attribute names as indicated in comments in the DAOs, you can generate the users and follow relationships by running the script from the command line using `npx ts-node <path to the script file name>`.

If your provisioned write capacity is high enough, it should not take very long, (at 200 WCUs it takes about 50 seconds per table) and you will see progress being indicated on the command line every few seconds as each 1,000 users or follow relationships are added.

Make sure you increase the WCU for the User table, the Follow table **and the index on the Follow table**.

You may also want to write code to clear the tables so you can empty them and try again if it fails. If you do that, you will need to make sure the WCUs are still high when you clear the tables. Another way to clear the tables is to drop and recreate them. That's actually faster than deleting the rows and won't require you to write additional logic. If you do drop and recreate the tables (just the User and Follow tables) don't forget to re-create the follow index.

If your script is taking a very long time to run, try increasing the write capacity units on your User and Follow tables and your follow index.

## How do I know it worked?

To verify that the tables filled with the correct number of items:

- View one of the tables (User or Follow) in the DynamoDB console.
- Click on the 'Overview' tab (you may need to press the 'View table details' button to make the tab visible).
- In about the middle of the page you should see an 'Items summary' section.
- Press the 'Get live item count' button.
- In the pop-up, click 'Start scan' or 'Scan again'.

When the 'Scan status' column says 'Complete' the 'Item count' will display the number of items currently in the table. If the number is smaller than you expected, check to make sure your script has finished running.

**REMEMBER: Be sure to lower your provisioned write capacity after your script finishes running. AWS will take your money away if you forget!**
