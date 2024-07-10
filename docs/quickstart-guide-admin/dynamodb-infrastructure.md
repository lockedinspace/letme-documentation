---
sidebar_position: 1
slug: dynamodb/infrastructure
---

# DynamoDB - Infrastructure

## Why we need a Database?

The first thing you may've thought is, **why do we need a database**? Well, this is simple. One of the objectives of letme is to **simplify**, **unify** and **centralize** the process of acquiring credentials. To do so, every user of the organization must know of:

- All the available accounts.
- All the possible roles.
- On multi-account role chaining, know all the hops in the right order.

If we distribute this knowledge through files, someday one **(if not all)** of the following situations will happen, **making our software unusable**:

- A user suddenly won't have access to an account because somebody changed the role name.
- My colleague has an account I don't have.
- Everybody on the team has custom names for every account, leading to confusion.
- Etc.

So, to solve these and other problems, we've found that using a database is the best solution.


## Why DynamoDB

We've chosen DynamoDB because it's:

- Fast.
- Reliable.
- Easy to use.
- Cost effective.

## Deploy a simple DynamoDB


:::danger 

**AWS services are not free**. Please refer to: **[DynamoDB Pricing Calculator](https://calculator.aws/#/createCalculator/DynamoDB)**

:::

Here's a CloudFormation template to deploy a simple DynamoDB on your account with `name` as the partition key **(required)**:

```yaml title="docs/aws/templates/dynamodb.yaml"
AWSTemplateFormatVersion: "2010-09-09"

Description: Creates a simple DynamoDB Table for Letme.

Parameters:
  PartitionKeyName: # DO NOT MODIFY
    Description: Name of the partition key
    Type: String
    Default: name

  PartitionKeyType: # DO NOT MODIFY
    Description: Type of the partition Key
    Type: String
    AllowedValues:
      - S
    Default: S

  ReadCapacityUnits:
    Description: Number of DynamoDB Read Capacity Units
    Type: Number
    Default: 1

  TableName:
    Description: DynamoDB table name.
    Type: String
    Default: cross-account-credentials

  WriteCapacityUnits:
    Description: Number of DynamoDB Write Capacity Units
    Type: Number
    Default: 1

Resources:
  DynamoDbTable:
    Type: AWS::DynamoDB::Table
    Properties:
      AttributeDefinitions:
        - AttributeName: !Ref PartitionKeyName
          AttributeType: !Ref PartitionKeyType
      ProvisionedThroughput:
        ReadCapacityUnits: !Ref ReadCapacityUnits
        WriteCapacityUnits: !Ref WriteCapacityUnits
      KeySchema:
        - AttributeName: !Ref PartitionKeyName
          KeyType: HASH
      TableName: !Ref TableName
```