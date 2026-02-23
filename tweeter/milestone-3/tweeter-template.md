```yaml
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: SAM config for Tweeter server


#
# Global Lambda function properties. These apply to every Lambda function.
#
Globals:
  Function:
    Runtime: nodejs20.x
    CodeUri: ./dist/
    Layers:
      - !Ref tweeterLayer
    Timeout: 30
    MemorySize: 128


#
# Reusable blocks of configuration. These are referenced throughout the template to avoid duplication.
# &identifier is used to define a reusable block, and *identifier is used to reference it.
#
Metadata:
  x-common-policies: &commonPolicies
    - AWSLambdaBasicExecutionRole
    - AmazonSESFullAccess
    - CloudWatchFullAccess
    - AmazonDynamoDBFullAccess
    - AmazonS3FullAccess
    - AmazonSQSFullAccess

  x-common-cors-headers: &commonCorsHeaders
    method.response.header.Access-Control-Allow-Origin: "'*'"

  x-common-swagger-response-headers: &commonSwaggerResponseHeaders
    Access-Control-Allow-Origin:
      type: string


#
# AWS resource definitions
#
Resources:


  #
  # Web API
  #

  tweeterApi:
    Type: AWS::Serverless::Api
    Properties:
      Name: tweeterApi
      StageName: prod
      Cors:
        AllowMethods: "'GET,POST,PUT,DELETE'"
        AllowHeaders: "'*'"
        AllowOrigin: "'*'"
      DefinitionBody:
        swagger: "2.0"
        info:
          title: "Tweeter API"
          version: "1.0"
        x-common-consumes: &commonConsumes
          - application/json
        x-common-produces: &commonProduces
          - application/json
        x-common-request-templates: &commonRequestTemplates
          application/json: $input.json('$')
        x-common-amazon-responses: &commonAmazonResponses
          default:
            statusCode: 200
            responseParameters: *commonCorsHeaders
          ".*bad-request.*":
            statusCode: 400
            responseParameters: *commonCorsHeaders
            responseTemplates:
              application/json: '{ "error": "$input.path(''$.errorMessage'')" }'
          ".*unauthorized.*":
            statusCode: 401
            responseParameters: *commonCorsHeaders
            responseTemplates:
              application/json: '{ "error": "$input.path(''$.errorMessage'')" }'
          ".*internal-server-error.*":
            statusCode: 500
            responseParameters: *commonCorsHeaders
            responseTemplates:
              application/json: '{ "error": "$input.path(''$.errorMessage'')" }'
        x-common-swagger-responses: &commonSwaggerResponses
          "200":
            description: "OK"
            headers: *commonSwaggerResponseHeaders
          "400":
            description: "Bad Request"
            headers: *commonSwaggerResponseHeaders
          "401":
            description: "Unauthorized"
            headers: *commonSwaggerResponseHeaders
          "500":
            description: "Internal Server Error"
            headers: *commonSwaggerResponseHeaders
        paths:

          #
          # Endpoint definitions
          #

          /user/get:  ##### TODO: Change "/user/get" to the actual endpoint URL
            post:
              consumes: *commonConsumes
              produces: *commonProduces
              x-amazon-apigateway-integration:
                type: aws
                httpMethod: POST
                uri:
                  ##### TODO: Change "userGetFunction" to the actual function name. (Don't delete the $, {}, or .Arn suffix.)
                  ##### The function name must agree with the "Web API Lambda functions" section below.
                  Fn::Sub: arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${userGetFunction.Arn}/invocations
                requestTemplates: *commonRequestTemplates
                responses: *commonAmazonResponses
              responses: *commonSwaggerResponses

          /user/create:  ##### TODO: Set endpoint URL
            post:
              consumes: *commonConsumes
              produces: *commonProduces
              x-amazon-apigateway-integration:
                type: aws
                httpMethod: POST
                uri:
                  ##### TODO: Set function name
                  Fn::Sub: arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${userCreateFunction.Arn}/invocations
                requestTemplates: *commonRequestTemplates
                responses: *commonAmazonResponses
              responses: *commonSwaggerResponses

            #
            # TODO: ADD MORE ENDPOINT DEFINITIONS HERE
            #


  #
  # SQS Queues  (This will become relevant in Milestone 4. For M3, leave this section commented out.)
  #

  #   MyQueue:
  #     Type: AWS::SQS::Queue
  #     Properties:
  #       QueueName: MyQueue


  #
  # Lambda Layer (reused by all Lambda functions)
  #
  tweeterLayer:
    Type: AWS::Serverless::LayerVersion
    Properties:
      LayerName: tweeterLayer
      Description: dependencies layer for Tweeter server
      ContentUri: ./layer/
      CompatibleRuntimes:
        - nodejs20.x
      RetentionPolicy: DELETE


  #
  # Web API Lambda functions
  #

  ##### TODO: Change "userGetFunction" to the actual function name.
  ##### It must match the function name in the "Endpoint definitons" section above.
  userGetFunction:
    Type: AWS::Serverless::Function
    Properties:
      ##### TODO: Change "userGetFunction" to the actual function name.
      FunctionName: userGetFunction
      ##### TODO: Modify Handler property to match your code. This identifies the exact function that the lambda should run.
      Handler: handlers/users/UserGetHandler.userGetHandler
      Policies: *commonPolicies
      Events:
        PostApi:
          Type: Api
          Properties:
            RestApiId: !Ref tweeterApi
            ##### TODO: Modify the Path property to the actual endpoint URL.
            ##### It must match the URL in the "Endpoint definitions" section above.
            Path: /user/get
            Method: POST

  userCreateFunction:  ##### TODO: Set function name
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: userCreateFunction  ##### TODO: Set function name
      Handler: handlers/users/UserCreateHandler.userCreateHandler  ##### TODO: Set Handler
      Policies: *commonPolicies
      Events:
        PostApi:
          Type: Api
          Properties:
            RestApiId: !Ref tweeterApi
            Path: /user/create  ##### TODO: Set Path
            Method: POST

  #
  # TODO: ADD MORE WEB API LAMBDA DEFINITIONS HERE
  #


  #
  # SQS Queue Lambda Functions  (This will become relevant in Milestone 4. For M3, leave this section commented out.)
  #

  #   MyQueueFunction:
  #     Type: AWS::Serverless::Function
  #     Properties:
  #       FunctionName: MyQueueFunction
  #       Handler: <PATH TO LAMBDA HANDLER>
  #       Policies: *commonPolicies
  #       Events:
  #         SQSTrigger:
  #           Type: SQS
  #           Properties:
  #             Queue: !GetAtt MyQueue.Arn
  #             Enabled: true


  #
  # DynamoDB Tables
  #

  visitTable:  ##### TODO: Change "visitTable" to match the table name. This doesn't
               #####       need to be the actual table name, just a unique identifier.
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: visit  ##### TODO: Set the actual table name. This is what your code will use to identify this table.
      AttributeDefinitions:
        ##### TODO: Add any attributes that will be used as partition or sort keys for the table or any indexes.
        - AttributeName: person_name
          AttributeType: S  ##### TODO: Set the data type for this attribute. S for string, N for number, etc.
        - AttributeName: place_name
          AttributeType: S
      KeySchema:
        - AttributeName: person_name  ##### TODO: Set the table partition key name
          KeyType: HASH
        - AttributeName: place_name  ##### TODO: Set the table sort key name. Delete these
          KeyType: RANGE             #####       two lines if the table has no sort key.
      BillingMode: PROVISIONED
      ProvisionedThroughput:
        ReadCapacityUnits: 1   ##### TODO: Set the table's Read and Write capacities. Leaving these at
        WriteCapacityUnits: 1  #####       one is fine for now, but you may need to change them later.
      ##### TODO: If you don't need an index for this table, delete the GlobalSecondaryIndexes property and all its subproperties.
      GlobalSecondaryIndexes:
        - IndexName: place_index  ##### TODO: Set the actual index name. This is what your code will use to identify this index.
          KeySchema:
            - AttributeName: place_name  ##### TODO: Set the index partition key name
              KeyType: HASH
            - AttributeName: person_name  ##### TODO: Set the index sort key name. Delete these
              KeyType: RANGE              #####       two lines if the index has no sort key.
          ProvisionedThroughput:
            ReadCapacityUnits: 1   ##### TODO: Same thing. Indexes have their own read/write
            WriteCapacityUnits: 1  #####       capacities that you may need to change.
          Projection:
            ProjectionType: ALL

  followTable:  ##### TODO: Change to match the table name
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: follow  ##### TODO: Set the actual table name
      AttributeDefinitions:
        ##### TODO: Add attributes that will be partition or sort keys, including their datatypes (for both table and index)
        - AttributeName: follower_alias
          AttributeType: S
        - AttributeName: followee_alias
          AttributeType: S
      KeySchema:
        - AttributeName: follower_alias  ##### TODO: Set table partition key
          KeyType: HASH
        - AttributeName: followee_alias  ##### TODO: Set table sort key
          KeyType: RANGE
      BillingMode: PROVISIONED
      ProvisionedThroughput:
        ReadCapacityUnits: 1
        WriteCapacityUnits: 1
      GlobalSecondaryIndexes:
        - IndexName: follow_index  ##### TODO: Set the actual index name
          KeySchema:
            - AttributeName: followee_alias  ##### TODO: Set index partition key
              KeyType: HASH
            - AttributeName: follower_alias  ##### TODO: Set index sort key
              KeyType: RANGE
          ProvisionedThroughput:
            ReadCapacityUnits: 1
            WriteCapacityUnits: 1
          Projection:
            ProjectionType: ALL

  #
  # TODO: ADD MORE TABLE DEFINITIONS HERE
  #
  
```
