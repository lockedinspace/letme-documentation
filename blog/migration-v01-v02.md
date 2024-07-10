---
slug: migration-guide/v0.2
title: "Migration guide: v0.2.0"
authors: 
  - hruiz
tags: [release, guide]
---


![](/img/letme-banner.webp)

<!--truncate-->
# Migration Guide

## Permissions

We've made some changes on the API Calls made to AWS to improve **performance** and **reliability**. So, **you need to update your permissions**.

:::info
**See**: [IAM Configuration](../quickstart-guide-admin/iam.md)
:::

## Letme cache

Before letme stored all DynamoDB items on `.letme/.letme-cache`. **This has been deprecated and must be removed.**

```bash
rm ~/.letme/.letme-cache
```

## Letme config file

In order to standarize the process of generating new credentials, we've changed all letme configuration to ini files. Therefore, we need to modify it:

1. Open `~/.letme/letme-config` file with your editor. You may have something like this:

```toml
[general]
  aws_source_profile = "onnera"
  aws_source_profile_region = "eu-west-1"
  dynamodb_table = "onnera_auth_cross-account-credentials-registry"
  mfa_arn = "arn:aws:iam::876842235607:mfa/MFA-ONNERA"
  session_name = "crodriguez_seidor"
```

2. Remove all `"` characters and align all text to the left side:

```ini
[general]
aws_source_profile = letme
aws_source_profile_region = eu-west-1
dynamodb_table = letme-accounts-table
mfa_arn = arn:aws:iam::1234567890:mfa/my-device
session_name = user_letme
```

## Letme active context

If you have renamed your context on `.letme/letme-config` file you must set it as **the active contexts**:

1. List all the avalaible contexts:

```bash
letme config get-contexts
```

2. Switch to your desired context:

```bash
letme config switch-context ${contextName}
```