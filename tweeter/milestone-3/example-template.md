```yaml
AWSTemplateFormatVersion: "2010-09-09"
Transform: AWS::Serverless-2016-10-31
Description: SAM app with TypeScript-based Lambdas, API Gateway, SQS, and DynamoDB

Globals:
  Function:
    Timeout: 10
    Runtime: nodejs18.x
    MemorySize: 128
    Tracing: Active

Resources:
  ### DynamoDB Table ###
  MyTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: MySampleTable
      AttributeDefinitions:
        - AttributeName: id
          AttributeType: S
      KeySchema:
        - AttributeName: id
          KeyType: HASH
      BillingMode: PAY_PER_REQUEST

  ### SQS Queue ###
  MyQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: MyQueue

  ### API Lambda Function ###
  ApiFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: api_function/
      Handler: dist/app.handler
      Events:
        ApiEndpoint:
          Type: Api
          Properties:
            Path: /hello
            Method: GET
      Environment:
        Variables:
          TABLE_NAME: !Ref MyTable

  ### SQS Lambda Function ###
  QueueFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: queue_function/
      Handler: dist/app.handler
      Events:
        SQSTrigger:
          Type: SQS
          Properties:
            Queue: !GetAtt MyQueue.Arn
      Environment:
        Variables:
          TABLE_NAME: !Ref MyTable
```
