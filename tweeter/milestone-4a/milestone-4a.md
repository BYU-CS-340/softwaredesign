# Project Milestone 4 Part A: Data Access Layer

In this milestone you will implement the rest of your server and complete all of the functionality described in the Course Project overview, including the requirements for authentication tokens and the handling of passwords, using DynamoDB to persist data rather than using hard-coded dummy data, and uploading user profile images to S3.

## Requirements

Add a data access layer to your server's design. Design and implement a set of DAO classes that support all of the database access needs of your server, as follows:

1. Define each DAO type using an interface that has no dependencies on a particular underlying database (i.e., the interfaces should be database agnostic). This will allow your server to be easily ported to a different database later if needed.
   - S3 should also have a DAO interface.
1. Modify your Service classes to use your DAOs to access the database (i.e., no more hard-coded dummy data).
1. Use the Abstract Factory pattern or Dependency Injection (not covered in class) to give your Service classes access to the DAO objects they need. Specifically, your Service classes should never call "new" to create a DAO, but instead call an abstract factory to create DAOs, or receive them through dependency injection. Your Service classes should be unaware of what particular database is being used.
1. Create a package of classes that implement your DAO interfaces using DynamoDB.
   - S3 should also be implemented using a DAO.
1. Design and create a set of DynamoDB tables and indexes to store all necessary data.
   - Your design should pre-compute the contents of every user's feed and store it in the database, rather than calculating user feeds dynamically. This will allow user feeds to be retrieved very quickly, and allow your server to meet the project performance requirements.
1. Your data access layer should also support the ability to upload user profile images to AWS S3. **(Look at the FAQ under "How do I upload my image to s3" for code on how to do this.)**
1. Your implementation should meet the "user and session management" requirements in the [project overview](../project-overview/tweeter.md). Articles about salting and hashing passwords can be found [here](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords) and [here](https://blog.logrocket.com/password-hashing-node-js-bcrypt/). We recommend using [bcryptjs](https://www.npmjs.com/package/bcryptjs) for hashing your passwords. The second article shows how to use bcrypt. **Make sure when you install the library, that you install bcryptjs, not the bcrypt library shown in the article.** The one you are told to install in the article does not work with AWS lambdas. Make sure you also install @types/bcryptjs.
   **Note:** Make sure you authenticate the user in every lambda. We recommend creating an AuthorizationService for this. Your service can throw an exception (which will be caught by API Gateway) if a user is not authorized. Make sure the exception message maps to a status code / regular expression mapping in API Gateway.
1. Avoid duplicating code by using inheritance, generic types, composition/delegation, Template Method pattern, Strategy pattern, passing functions as parameters, etc.
1. At least one tab should be populated with more than 10 items so we can test the scrolling behavior.
1. Make sure that error responses work. Now that we no longer use FakeData, this can be tested. The easiest to test is login. CORS will need to be enabled on the error responses, see [Milestone 3: API Design and Implementation](../milestone-3/milestone-3.md) for details on how.

After completing Part A, you should have a fully functional system (client and server). However, your server will probably not meet all of the project performance requirements. This will be rectified in Part B.

**Note:** It is NOT required that tabs update dynamically while using the application. For example, if you follow or unfollow a user while the Followees tab is selected, it is not required that the change is immediately displayed in the Followees tab. However, if you navigate away from and then back to the Followees tab, the change should be displayed. Similarly, if you post a status while the Story tab is displayed, it is not required that the status appear in the Story tab until you navigate away from and then back to the Story tab.

## DynamoDB Notes

### DynamoDB Provisioned Capacity

- No matter what architecture you develop, your performance will be capped by the capacity you provision for writing to the feed table in DynamoDB.
- To learn about provisioned capacity for reads (RCUs), read the following: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html#ItemSizeCalculations.Reads
- To learn about provisioned capacity for writes (WCUs), read the following: https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughput.html#ItemSizeCalculations.Writes
- Batch writes will be more efficient than individual put-items, because you will have fewer network round trips. A batch-write operation is limited to 25 items. If you include 25 items, you will consume 25 write capacity units, as long as each item is no more than 1 KB in size.

### Minimizing AWS charges

There may be a small charge associated with this milestone, but to minimize this consider the following:

**The DynamoDB free usage tier is that your total RCU and total WCU both are 25 or under. If you exceed 25 WCU or RCU, you will be charged some amount. You can turn up WCU and RCU while testing and passing off (if needed) but turn it down otherwise to avoid getting charged for capacity you are not using.**

## AWS Notes

[Some gotchas for AWS and tips to avoid getting charged $$$](../project-overview/aws-account.md)

### Uploading images to S3

In order to upload your image string to S3, you will need to convert the string to a byte array and send the request to S3 using a Put Object Request. You can do this using the following code:

```typescript
  async putImage(
    fileName: string,
    imageStringBase64Encoded: string
  ): Promise<string> {
    let decodedImageBuffer: Buffer = Buffer.from(
      imageStringBase64Encoded,
      "base64"
    );
    const s3Params = {
      Bucket: BUCKET,
      Key: "image/" + fileName,
      Body: decodedImageBuffer,
      ContentType: "image/png",
      ACL: ObjectCannedACL.public_read,
    };
    const c = new PutObjectCommand(s3Params);
    const client = new S3Client({ region: REGION });
    try {
      await client.send(c);
      return (
      `https://${BUCKET}.s3.${REGION}.amazonaws.com/image/${fileName}`
      );
    } catch (error) {
      throw Error("s3 put image failed with: " + error);
    }
  }
```

image_string is the string of bytes that should be getting passed to your server code. Swap the name "student-bucket" with your S3 bucket name in both the URL and PutObjectRequest. Swap region-name in the URL with the region your S3 bucket is created in. Additionally, you may need to change a couple of settings in your S3 bucket to get this to work. Ensure the following settings are changed so your S3 bucket has public access:

Permissions -> Block Public Access needs to block nothing.
Permissions -> Object Ownership needs to allow ACLs.

Make sure that your register lambda also has full access to S3 by going to configuration->permissions and viewing the lambda role.

## Passoff

- **Pass off your project with a TA by the due date at the end of TA hours (you must be in the pass off queue 1 hour before the end of TA hours to guarantee pass off)**
- You can only passoff once.
- If you passoff before the passoff day, you will get an additional 4% of extra credit in this assignment

## Submission

You do not need to submit any files for this milestone, just passoff with a TA before the due date as stated above.

## Debugging

Also check out [this page](../milestone-3/debugging-tips.md) for network debugging tips.

## Rubric

- [25 pts] For well-designed DAO interfaces, DAOs that correctly write to DynamoDB, using abstract factory or dependency injection.

- [10 pts] For proper database structure (Tables and S3).

- [10 pts] For authentication and session management that meet requirements.

## [Milestone 4 FAQ](./milestone-4-faq.md)
