```
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

          /user/get:
            post:
              consumes: *commonConsumes
              produces: *commonProduces
              x-amazon-apigateway-integration:
                type: aws
                httpMethod: POST
                uri:
                  ##### Change "userGetFunction" to the actual function name.
                  Fn::Sub: arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${userGetFunction.Arn}/invocations
                requestTemplates: *commonRequestTemplates
                responses: *commonAmazonResponses
              responses: *commonSwaggerResponses

          /user/create:
            post:
              consumes: *commonConsumes
              produces: *commonProduces
              x-amazon-apigateway-integration:
                type: aws
                httpMethod: POST
                uri:
                  ##### Change "userCreateFunction" to the actual function name.
                  Fn::Sub: arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${userCreateFunction.Arn}/invocations
                requestTemplates: *commonRequestTemplates
                responses: *commonAmazonResponses
              responses: *commonSwaggerResponses

            #
            # ADD MORE ENDPOINT DEFINITIONS HERE
            #


  #
  # SQS Queues
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

  userGetFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: userGetFunction
      ##### Modify Handler property to match your code.
      Handler: handlers/users/UserGetHandler.userGetHandler
      Policies: *commonPolicies
      Events:
        PostApi:
          Type: Api
          Properties:
            RestApiId: !Ref tweeterApi
            Path: /user/get
            Method: POST

  userCreateFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: userCreateFunction
      ##### Modify Handler property to match your code.
      Handler: handlers/users/UserCreateHandler.userCreateHandler
      Policies: *commonPolicies
      Events:
        PostApi:
          Type: Api
          Properties:
            RestApiId: !Ref tweeterApi
            Path: /user/create
            Method: POST

  #
  # ADD MORE WEB API LAMBDA DEFINITIONS HERE
  #


  #
  # SQS Queue Lambda Functions
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
  followTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: follow
      AttributeDefinitions:
        - AttributeName: follower_alias
          AttributeType: S
        - AttributeName: followee_alias
          AttributeType: S
      KeySchema:
        - AttributeName: follower_alias
          KeyType: HASH
        - AttributeName: followee_alias
          KeyType: RANGE
      BillingMode: PAY_PER_REQUEST
      GlobalSecondaryIndexes:
        - IndexName: follow_index
          KeySchema:
            - AttributeName: followee_alias
              KeyType: HASH
            - AttributeName: follower_alias
              KeyType: RANGE
          Projection:
            ProjectionType: ALL

  #
  # ADD MORE TABLE DEFINITIONS HERE
  #
  
```
