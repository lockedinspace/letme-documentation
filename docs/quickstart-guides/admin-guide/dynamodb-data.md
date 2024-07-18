---
sidebar_position: 2
slug: /guide/admin/dynamodb/data
---

# DynamoDB - Data

Items JSON structure and how to configure Assume Role and multi-account Role Chaining items.

## JSON Structure

Every JSON file on a DynamoDB is an Item. Letme needs every item to have at least the following fields:

| **Fields**    | **Type**         | **Description**                                             |
| ------------- | ---------------- | ----------------------------------------------------------- |
| `name`        | String           | Alias or common name used to identify an AWS account.       |
| `region`      | Array of strings | AWS Region code where CLI requests will be made by default. |
| `role`        | Array of strings | IAM Role/s ARN that will be assumed by `letme`.             |
| `tags`        | Array of strings | Labels to filter results on list.             |

:::tip
**See:** [Support for filtering and json output](/blog/release/0.2.1)
:::


### Assume Role JSON example

This JSON allows us to obtain `assume-role-account` credentials using `simple-role` role.

```json title="docs/aws/templates/assume-role-item.json"
{
    "name": "lockedinspace",
    "region": [
     "eu-west-1"
    ],
    "role": [
     "arn:aws:iam::123456789012:role/simple-role"
    ]
}
```

:::tip
To **understand Assume Role**, see: [**How it works - Assume Role**](/technical-guide/how)
:::

:::tip
To know **how to setup Assume Role**, see: [**IAM Roles**](./iam.md)
:::

### Multi-account role chaining JSON example

Multi Account role chaining consists of assuming a role through a series of IAM Roles. This JSON allows us to obtain `lockedinspace_chained` and get credentials from role `arn:aws:iam::456789987652:role/simple-role-third-account`:

```json title="assume-role-chained-item.json"
{
    "name": "lockedinspace_chained",
    "region": [
     "eu-west-1"
    ],
    "role": [
     "arn:aws:iam::123456789012:role/simple-role",
     "arn:aws:iam::098765432112:role/simple-role-second-account",
     "arn:aws:iam::456789987652:role/simple-role-third-account"
    ]
}
```



:::tip
To **understand multi-account role chaining**, see: [**How it works - Multi-account role chaining**](/technical-guide/how#multi-account-assume-role-chaining)
:::

:::tip
To know **how to setup multi-account role chaining**, see: [**IAM Roles**](./iam.md)
:::