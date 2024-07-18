---
slug: release/0.2.1
title: "Release v0.2.1: List filtering and json output"
authors: 
  - hruiz 
tags: [release]
---

# What's New 

Here we're with a new version that comes new features for list command!

<!--truncate-->

## Support for JSON output

Now you can specify to list command to output results in JSON format:

```bash
❯ letme list -o json
{
 "items": [
  {
   "name": "grabber",
   "region": "eu-west-1"
  },
  {
   "name": "lockedinspace",
   "region": "eu-west-1"
  }
 ]
}
```

## Support for filtering

### Putting it in context
This solution is focused to solve a situation where **AWS administrators may have different roles for different types of users**. 

For example, this table represents some users that have different permission on AWS Account `1234567890`:

| **User** | **Group**      | **AccountID Target** | **RoleName to assume** |
|----------|----------------|----------------------|------------------------|
| Robben   | SRE            | 1234567890           | sre_ops                |
| Maria    | FinOps         | 1234567890           | finops                 |
| Manuel   | ProjectManager | 1234567890           | read_only              |


On our DynamoDB we should see something like this:

| **Name** | **Region**     | **Role**                               | 
|----------|----------------|----------------------------------------|
| sre_ops  | eu-west-1      | arn:aws:iam::1234567890:role/sre_ops   |
| finops   | FinOps         | arn:aws:iam::1234567890:role/finops    |
| manager  | ProjectManager | arn:aws:iam::1234567890:role/read_only | 

### How we handled it

Before when you executed `letme list` you saw all the available profiles, **even the ones you can't assume**!

```bash
> letme list
NAME:       MAIN REGION:
-----       ------------
sre_ops     eu-west-1
finops      eu-west-1
manager     eu-west-1
```

### Now

To fix this situation, we've added support to a new field called tags:

:::info
**See:** [DynamoDB Data](/guide/admin/dynamodb/infrastructure) 
:::

With tags you can define keywords to filter output. This is an example of the previous DynamoDB with tags:

| **Name** | **Region**     | **Role**                               | *Tags*  | 
|----------|----------------|----------------------------------------|---------|
| sre_ops  | eu-west-1      | arn:aws:iam::1234567890:role/sre_ops   | sre     |
| finops   | FinOps         | arn:aws:iam::1234567890:role/finops    | finops  |
| manager  | ProjectManager | arn:aws:iam::1234567890:role/read_only | manager | 

Then we have two approaches:
#### Add `--filter` flag

We can add a flag to the list command to filter results:

```bash
❯ letme list --filter=sre
Listing accounts using 'lockedinspace' context:

NAME:       MAIN REGION:
-----       ------------
sre_ops     eu-west-1
```

#### Modify `~/.letme/letme-config` file

Another option is to modify your config file, this way you won't have to add `--filter` flag every time you do a list:

```ini
[lockedinspace]
aws_source_profile        = letme
aws_source_profile_region = eu-west-1
dynamodb_table            = letme-accounts-table
session_name              = letme_session
session_duration          = 3600
tags                      = sre
```

```bash
❯ letme list
Listing accounts using 'lockedinspace' context:

NAME:       MAIN REGION:
-----       ------------
sre_ops     eu-west-1
```
