# Project Milestone 4 Part B: Scalable Status Processing

In this milestone you will implement the rest of your server and complete all of the functionality described in the [Course Project](../project-overview/tweeter.md) overview, including the requirements for authentication tokens and the handling of passwords, using DynamoDB to persist data rather than using hard-coded dummy data, and uploading user profile images to S3.

## Populating Your Database With Test Data

For testing and passing-off, you will need to create ~10,000 test users and add data to your follows table such that you have at least one user with 10,000 or more followers. To do this, you will need a script that can create the users and add followers to your follows table. The generated users need not have profile pictures, or they could all have the same profile picture (do whatever makes sense for your implementation).

When you run your script/program to populate your database, you will probably need to temporarily increase the WCU settings on your users and follows tables (e.g., to 200). Once the test users and followers have been created, decrease the WCU settings on these tables back to their original levels (e.g., to 1 or 5).

Refer to the [DynamoDB Batch Write Example](../milestone-4a/dynamodb-batch-write-example.md) for help in writing your batch load script.

## Requirements

### Scalable Status Processing

You are to design your system to meet the following performance requirements:

1. The perceived latency of the create new status operation (from the perspective of the author) is to be less than 1 second.
1. When a new status is created, that status is visible in the feeds of all of the followers of the author within 120 seconds, for authors with up to 10K followers.
1. Each page of a user's feed is returned in less than 1 second, from the perspective of the user.

To meet these performance requirements you are to do the following:

1. When a new status is posted, the feed of each follower is updated. (That is feeds are updated at write-time rather than assembled at read-time.)
1. Use two AWS SQS queues for asynchronously processing feed updates.
1. Your feed table is to be configured with no more than 100 WCUs.

This [diagram](/tweeter/milestone-4b/PostStatusArchitectureDiagram-M4B.pdf) shows the post status feature architecture that will allow you to meet these requirements. This [diagram](/tweeter/milestone-4b/PostStatusActivityFlowDiagram-M4B.pdf) shows the same information but in the form of an activity/flow diagram.

We will discuss these techniques further in class.

### Automated Testing

#### New Automated Testing

Using the jest testing framework and ts-mockito for spying/mocking, write an automated integration test to verify that when a user sends a status, the status is correctly appended to the user's story. Your test should do the following:

1. Login a user. [This can be done by directly accessing the ServerFacade or client side service class]
1. Post a status from the user to the server by calling the "post status" operation on the relevant Presenter.
1. Verify that the "Successfully Posted!" message was displayed to the user.
1. Retrieve the user's story from the server to verify that the new status was correctly appended to the user's story, and that all status details are correct. [This can be done by directly accessing the ServerFacade or client side service class]

#### Old Automated Testing

Ideally, test cases will work as you add features. In Milestone 3, you created tests that relied on Fake Data being returned from your backend but now that you have real data these tests likely no longer work. In the real world, you would update these tests so that they continue to work. For the sake of reducing the time requirements of this milestone, you may delete these tests (or comment them out/ignore them)

## DynamoDB Notes

### DynamoDB Provisioned Capacity

- No matter what architecture you develop, your performance will be capped by the capacity you provision for writing to the feed table in DynamoDB.
- To learn about provisioned capacity for reads (RCUs), read the following: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html#ItemSizeCalculations.Reads
- To learn about provisioned capacity for writes (WCUs), read the following: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html#ItemSizeCalculations.Writes
- Batch writes will be more efficient than individual put-items, because you will have fewer network round trips. A batch-write operation is limited to 25 items. If you include 25 items, you will consume 25 write capacity units, as long as each item is no more than 1 KB in size.
- Updating the feeds when a status is posted by an author that has 10,000 followers will require writing 10,000 items, possibly in 400 batches (10000/25). To write that many items in 120 seconds will require WCU setting of around 10,000/120.

### Minimizing AWS charges

**There may be a small charge associated with this milestone, but to minimize this consider the following:**

- **Turn up the DynamoDB WCUs for your feed table while testing and passing-off, but turn it down otherwise to avoid getting charged for capacity you are not using. Also, be sure to turn down the WCU settings on your users and follows tables after your test data has been generated (as previously mentioned).**

## AWS Notes

[Some gotchas for AWS and tips to avoid getting charged $$$](../project-overview/aws-account.md)

## Passoff

- **Pass off your project with a TA by the due date at the end of TA hours (you must be in the pass off queue 1 hour before the end of TA hours to guarantee pass off)**
- You can only passoff once.
- If you passoff before the passoff day, you will get an additional 4% of extra credit in this assignment

## Debugging

At this point you should be pretty good at debugging your code, but don't forget about [this article](../milestone-3/debugging-tips.md) if you need any tips.

## Rubric

- [25 pts] For meeting the three performance requirements listed above, while using no more than 100 WCUs on the feed table. For full points, you must update feeds asynchronously when a status is created and must use SQS.

- [15 pts] For implementing the specified automated tests.

## [Milestone 4 FAQ](../milestone-4a//milestone-4-faq.md)
