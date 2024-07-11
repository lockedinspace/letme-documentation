---
sidebar_position: 3
slug: iam
---

# IAM

On this section we'll explain how to setup IAM to be able to do simple Assume Role and multi-account assume role chaining. 

## Auth account

On AWS, IAM **Users don't have permissions** to do anything by default. Therefore, we must grant users at least this permissions:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PermissionToAssumeRoleFromOtherAccounts",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": [
                "arn:aws:iam::${AWS_ACCOUNT_ID_1}:role/role",
                "arn:aws:iam::${AWS_ACCOUNT_ID_2}:role/role2"
                // ...
            ]
        },
        {
            "Sid": "PermissionToGetScanItemsFromDynamoDB",
            "Effect": "Allow",
            "Action": [
                "dynamodb:GetItem", 
                "dynamodb:Scan"
                "dynamodb:ListTables"
            ],
            "Resource": "arn:aws:dynamodb:${AWS_REGION}:${AWS_ACCOUNT_ID}:table/${TABLE_NAME}"
        }
    ]
}
```
:::info
Modify variables according to your AWS Account Setup.
:::

## Target Accounts

### Assume Role

To be able to do simple Assume Roles, IAM Roles must allow sts:AssumeRole on their trust relationship. For example:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PermissionToAssumeRole2",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::${AWS_ACCOUNT_ID}:user/letme"
        }
    ]
}
```

## Multi-account assume role chaining

On multi-account assume role chaining things get more complex. Here's a diagram to better understand the configuration:

![](/img/assume-role-chained.png)

1. First, we need to grant permissions defined above in [Auth Account](./iam.md#auth-account).
2. Then, we need to allow on the **trust relationship** `sts:AssumeRole` from the ((Nominal User)) (letme) on **Account 1** role (first_role):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PermissionToAssumeRole2",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::${AUTH_ACCOUNT}:user/letme"
        }
    ]
}
```
2. Finally, on **Account 2** we need to allow on the **trust relationship** `sts:AssumeRole` from the role in Account 1 (first_role):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PermissionToAssumeRole2",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::${ACCOUNT_1}:role/first_role"
        }
    ]
}
```

:::tip
To **understand multi-account role chaining**, see: [**How it works - Multi-account role chaining**](#multi-account-assume-role-chaining)
:::