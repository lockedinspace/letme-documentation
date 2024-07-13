---
sidebar_position: 3
slug: config-file
---

# The configuration file

[1]: https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html
[2]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html
[3]: ../quickstart-guide-admin/dynamodb-infrastructure.md
[4]: https://docs.aws.amazon.com/cli/latest/reference/iam/list-mfa-devices.html
[5]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html
[6]: https://awscli.amazonaws.com/v2/documentation/api/2.0.33/reference/sts/assume-role.html#options
[7]: ../quickstart-guide-admin/dynamodb-data.md


Letme holds a configuration file on `~/.letme/letme-config`. It stores details about your AWS configuration.

## Management

Letme configuration file is managed through `letme config` command.

:::info
**See:** [letme config command](../letme-usage/config.md)
:::

## Available configuration

When you create a new context, you'll have a config file like this:

```ini
[general]
aws_source_profile = letme
aws_source_profile_region = eu-west-1
dynamodb_table = letme-accounts-table
mfa_arn = arn:aws:iam::1234567890:mfa/my-device
session_name = user_letme
```
:::tip
**To create a new context run:** `letme config new-context ${contextName}`
:::

Here's the full list of avalaible fields:

| Key | Description | Default value | Required | Type |
| ------ | ------ | ------ | ------ | ------ |
| ``aws_source_profile`` | The AWS CLI profile name which maps to the source account. This profile must held the DynamoDB table. [\[1\]][1] | ``default`` | No | ``string`` |
| ``aws_source_profile_region`` | The region name in the source account where the DynamoDB table is located [\[2\]][2] | ``-`` | Yes | ``string`` |
| ``dynamodb_table`` | The DynamoDB table name where the AWS accounts are stored [\[3\]][3] | ``-`` | Yes | ``string`` |
| ``mfa_arn`` | Virtual MFA device arn used to authenticate against AWS [\[4\]][4]  | ``-`` | No (depending on your AWS trust relationship policy) | ``string`` |
| ``session_duration`` | Credentials duration time in seconds [\[5\]][5]| `3600` | No | ``number`` |
| ``session_name`` | The session name when performing assumeRole requests [\[7\]][7]| `${account_name}-letme-session` | No | ``string`` |
| ``tags`` | Comma delimited list of values to filter accounts [\[6\]][6]| `-` | No | `[]string` |


:::info
`-` represents an empty or null value.
:::