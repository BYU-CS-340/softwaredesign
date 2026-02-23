# Server Scripts

Since there are 14 lambdas in milestones 3 and 4A, and 16 in milestone 4B, uploading and updating the lambda and lambda layer code for each lambda manually can be tedious. These scripts will speed up the process. You are welcome to use them "as is" or modify them to suit your needs.

Before using the scripts you will need to do the following setup in AWS:

1. Create an S3 bucket in the region where you want to deploy your lambdas. Your bucket will need to have a name that is globally unique. Create a folder in the bucket named 'code'.
1. Create an AWS role using IAM and give it the following permissions:
   - AmazonS3FullAccess
   - AWSLambda_FullAccess
   - CloudWatchLogsFullAccess
   - AmazonDynamoDBFullAccess
   - AmazonSQSFullAccess
1. Create a lambda layer in AWS (in the same region as step 1) containing the dependencies for your lambda code. Follow the instructions in the Lambda in-class exercise.

Now create a .server file at the root of your server module with the following contents:

```txt
BUCKET='TODO: Enter your bucket name here'
LAMBDA_ROLE='TODO: Enter the ARN of your lambda role here'
EDIT_LAMBDALIST='
TODO: Enter a newline separated list of lambda names and corresponding lambda function names as follows:
tweeterGetFollowees | lambda/follow/GetFolloweesLambda.handler
'
LAMBDALAYER_ARN='TODO: Enter the ARN of your lambda layer here'
```

Replace the TODOs with the appropriate information. You can get the ARNs by navigating to the AWS resources you created above using the AWS console. Here's an example of what a .server file looks like with information for one lambda:

```txt
BUCKET='my_bucket'
LAMBDA_ROLE='arn:aws:iam::472934529729:role/tweeter-lambda'
EDIT_LAMBDALIST='
tweeterGetFollowees | lambda/follow/GetFolloweesLambda.handler
'
LAMBDALAYER_ARN='arn:aws:lambda:us-west-2:2875402752789:layer:tweeterLambdaLayer:1'
```

**Note:** You will need to create a new version of your lambda layer, upload the updated lambda layer code, and update the LAMBDALAYER_ARN in the .server file each time you change a dependency of either the server module or the shared module.

**Note:** When you have multiple lambda entries in the EDIT_LAMBDALIST, each entry is separated by a newline but not a comma.

The following code is a script that will create/deploy to AWS or update each lambda referenced in your .server file. Create a file named uploadLambdas.sh at the root of your server module and add the following code:

```bash
#!/bin/bash

# use set -e to terminate the script on error
set -e

source .server

aws s3 cp dist.zip s3://$BUCKET/code/lambdalist.zip

# using -e let's us use escape characters such as \n if the output is in quotation marks
echo -e '\n\n\nlambdalist.zip uploaded to the bucket. Updating or creating lambda functions...\n'

# Update or create Lambdas
i=1
PID=0
pids=()
IFS=$'\n' # Set IFS to newline to handle function name and handler pairs properly
for lambda_info in $EDIT_LAMBDALIST
do
    # Extract function name and handler from lambda_info and trim whitespace
    function_name=$(echo "$lambda_info" | cut -d '|' -f 1 | tr -d '[:space:]')
    handler=$(echo "$lambda_info" | cut -d '|' -f 2 | tr -d '[:space:]')

    if aws lambda get-function --function-name "$function_name" &>/dev/null; then
        # Lambda exists, update code
        aws lambda update-function-code \
            --function-name  "$function_name" \
            --s3-bucket $BUCKET \
            --s3-key code/lambdalist.zip \
            1>>/dev/null \
            &
        echo lambda $i, "$function_name", updating from s3
    else
        # Lambda doesn't exist, create it
        aws lambda create-function \
            --function-name "$function_name" \
            --runtime nodejs20.x \
            --role $LAMBDA_ROLE \
            --handler "$handler" \
            --code S3Bucket=$BUCKET,S3Key=code/lambdalist.zip \
            1>>/dev/null \
            &
        echo lambda $i, "$function_name", created from s3
    fi
    pids[${i-1}]=$!
    ((i=i+1))
done

# Wait for each process to finish
for pid in "${pids[@]}"; do
    wait "$pid"
done

echo -e '\nLambda functions updated or created.'
```

When you are ready to deploy or redeploy lambdas, create a zip file named dist.zip containing the lambda code as described in the lambda in-class exercise and execute the script with the following command: `./uploadLambdas.sh`

**Note:** The zip file should be created by zipping the contents of your dist folder, not the dist folder (i.e. when creating the zip, highlight the contents of dist and zip that instead of zipping the dist folder).

If the script does not have executable permissions, set them using `chmod +x uploadLambdas.sh` on Mac or Linux. On Windows this might not be necessary.

Each deployed lambda will need to reference the lambda layer containing the node_modules folder dependencies. Create an updateLayers.sh script for updating the lambda layers with the following code:

```bash
# Use this file to update the lambda layers for each lambda.
# First create the new lambda layer, or lambda layer version in aws by uploading the new lambda layer code.
# Then copy the arn for the lambda layer from aws to the .server LAMBDALAYER_ARN variable.
# Then run this script.

source .server

i=1
PID=0
pids=()
IFS=$'\n' # Set IFS to newline to handle function name and handler pairs properly
for lambda_info in $EDIT_LAMBDALIST
do
    # Extract function name and handler from lambda_info and trim whitespace
    function_name=$(echo "$lambda_info" | cut -d '|' -f 1 | tr -d '[:space:]')

    # Check if function_name is empty
    if [ -z "$function_name" ]; then
        continue
    fi

    aws lambda update-function-configuration --function-name "$function_name" --layer "$LAMBDALAYER_ARN" 1>>/dev/null &
    echo lambda $i, $function_name, updating lambda layer...
    pids[${i-1}]=$!
    ((i=i+1))
done

# Wait for each process to finish
for pid in "${pids[@]}"; do
    wait "$pid"
done

echo Lambda layers updated for all lambdas in .source
```

Make sure this file has executable permissions as well.

Before running this script, make sure you have created a lambda layer or new lambda layer version in AWS using the console and update the LAMBDALAYER_ARN variable in the .server file. This script associates the lambda layer with each lambda referenced in the .server file. You should run it each time you deploy a new lambda using the uploadLambda.sh script. You should also run it if you update the project dependencies and create a new lambda layer or lambda layer version.

Run the script with this command: `./updateLayer.sh`.
