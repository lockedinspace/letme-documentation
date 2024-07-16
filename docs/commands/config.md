---
sidebar_position: 2
slug: /commands/config
---

# Config

Lets you interact with contexts defined in `.letme/letme-config`.

## Commands

- List contexts:

```bash
letme config get-contexts
```

- Create a new context:

```bash
letme config new-context
```

- Change your active context:

```bash
letme config switch-context ${contextName}
```

- Update an existing context:

```bash
letme config update-context
```

- Verify `~/letme/.letme-config` is valid:

```bash
letme config validate
```

- Output a context config example:

```bash
letme config view-template
```

The following list depicts the letme-config (configuration file) fields:

| Key | Description | Default value | Required | Type |
| ------ | ------ | ------ | ------ | ------ |
| ``aws_source_profile`` | The AWS CLI profile name which maps to the source account. This profile AWS account must contain the DynamoDB table.  | ``default`` | No | ``string`` |
| ``aws_source_profile_region`` | The region name in the source account where the DynamoDB table is located  | ``-`` | Yes | ``string`` |
| ``dynamodb_table`` | The DynamoDB table name where the AWS accounts are stored  | ``-`` | Yes | ``string`` |
| ``mfa_arn`` | Virtual MFA device arn used to authenticate against AWS   | ``-`` | No (depending on your AWS trust relationship policy) | ``string`` |
| ``session_duration`` | Credentials duration time in seconds | `3600` | No | ``number`` |
| ``session_name`` | The session name when performing assumeRole requests  | `${account_name}-letme-session` | No | ``string`` |
| ``tags`` | Comma delimited list of values to filter accounts | `-` | No | `[]string` |


:::info
`-` represents an empty or null value.
:::