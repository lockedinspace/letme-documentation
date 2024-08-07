---
sidebar_position: 2
slug: /guide/user/config-file
---

# The configuration file

Configure your letme experience.

Your configuration file can be found at `~/.letme/letme-config`. It stores details about your AWS contexts.

## Concepts

Contexts are meant to be isolated AWS organizations or accounts which do not have any relationship between.
Suppose you are working with two different organizations, ``alpha`` and ``bravo``. 

Creating each context will enable us to differentiate between workloads with ease:
```bash
❯ letme config new-context alpha
...
❯ letme config new-context bravo
...
```
With this, we can easily switch between organizations with:
```bash
❯ letme config switch-context bravo
Using: 'bravo' context.

❯ letme list
Listing accounts using 'bravo' context:

NAME:                               MAIN REGION:
-----                               ------------
bravo1                               eu-west-1
bravo2                               eu-west-1
bravo-dr-account                     eu-west-1
```
## Management

Your configuration file is under the `letme config` command and subcommands.

:::info
**See:** [letme config command](../../commands/config.md)
:::

## Available configuration

When you create a context, what you are really doing is creating an entry in your configuration file similar to this one:

```ini
[bravo]
aws_source_profile = bravo_source_profile
aws_source_profile_region = ap-south-1
dynamodb_table = bravo_accounts_table
mfa_arn = arn:aws:iam::1234567890:mfa/my-device
session_name = user_letme_bravo
```
:::tip
**Create a new context with:** `letme config new-context ${contextName}`
:::
