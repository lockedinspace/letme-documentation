---
sidebar_position: 1
slug: install
---
# Install and set up letme

Seamesly switch between AWS accounts.

## Prerequisites
 - Go >= 1.22
 - AWS CLI >= 2.x.x
 - Git

## Installation Methods
  - [Through go cli (recommended)](#go-cli)
  - [Building from source](#building-from-source)
  
### Go CLI

Install the latest letme version with:

```bash
go install github.com/lockedinspace/letme@latest
```
:::info
[Where does go install the binary?](https://pkg.go.dev/cmd/go#hdr-Compile_and_install_packages_and_dependencies)
:::

You can also install a specific version swapping ``@latest`` with your desired version.

Available versions can be found as tags in the [letme official repo](https://github.com/lockedinspace/letme). 


### Building from source

Clone the repository

```bash
git clone git@github.com:lockedinspace/letme.git
```

Change directory to letme and build the binary:

```bash
cd letme/
go build 
```

Move the ``letme`` binary to one of your ``$PATH`` (linux-macos) / ``$env:PATH`` (windows-poweshell) locations.

## Setting up letme

If your organization has already created the necessary infrastructure for letme, you probably just want to
connect your letme client to your organization. 

If no organization exists, **you must first create the necessary resources**.

:::info
**See**: [Quickstart guide - AWS Administrators](../quickstart-guide-admin/dynamodb-infrastructure.md)
:::

### Connect letme to your organization and obtain credentials

Your organization administrator should provide you some values. You will need those values to complement your configuration file. To create it run:

```bash
❯ letme config new-context ${contextName}
```
Here's an example:
```bash
❯ letme config new-context lockedinspace
letme: creating/updating context 'lockedinspace'. Optional fields can be left empty.
→ AWS Source Profile Name: letme
→ AWS Source Profile Region: eu-central-1
→ AWS DynamoDB Table Name: letme-accounts-table
→ AWS MFA Device arn (optional): arn:aws:iam::1234567890:mfa/my-device
→ Token Session Duration in seconds (optional): 
→ Session Name (optional): user_letme
Created letme 'lockedinspace' context.
```

Now, if you open `~/.letme/letme-config` file you should see something like this:

```ini
[general]
aws_source_profile = letme
aws_source_profile_region = eu-west-1
dynamodb_table = letme-accounts-table
mfa_arn = arn:aws:iam::1234567890:mfa/my-device
session_name = user_letme
```
:::tip
**To get your MFA arn, run**: 
```bash
aws iam list-mfa-devices --query 'MFADevices[].SerialNumber'
```
:::


## Validate

Now you will be able to:

- List the accounts under your organization:
```bash
$ letme list
Listing accounts using 'general' context
NAME:                    MAIN REGION:
-----                    ------------
eu-client-1              eu-west-1
eu-client-3              eu-west-3
pro-landing-zone-ap      ap-south-1
uat-backups_storage      eu-west-1
```

- Obtain any account credentials with you are able to:

```bash
$ letme obtain pro-landing-zone-ap
Assuming role with the following session name: 'user_letme' and context: 'general'
Enter MFA one time pass code: 189575 
letme: use the argument '--profile pro-landing-zone-ap' to interact with the account.
```

- And perform AWS cli operations against that account:

```bash
$ aws s3 ls --profile pro-landing-zone-ap
2024-11-17 19:29:09 xxx-user-emails
2021-03-15 09:58:27 cf-templates-21581238ujt23
2019-06-07 15:53:14 elasticbeanstalk-eu-central-1-3582131231
2019-06-06 20:25:19 elasticbeanstalk-us-west-2-3582131231
2023-06-22 23:24:00 erp-xxxx-content
```