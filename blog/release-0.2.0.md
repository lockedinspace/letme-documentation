---
slug: release/0.2
title: "Release v0.2: New Horizons"
authors: hruiz
tags: [release]
---

# What's New 

Here we're with a new version that comes new features and base code improvement.

![](/img/letme-banner.webp)

<!--truncate-->

# New Features

Here's a list of what's been added to Letme.

## Multiple contexts

Before we had to have multiple `~/.letme/letme-config` files to be able to have different DynamoDB tables. Now, we can do so by switching contexts. 

For example, here's a `~/.letme/letme-config` file with multiple contexts:

```ini
[letme]
aws_source_profile        = letme
aws_source_profile_region = eu-west-1
dynamodb_table            = cross-account-credentials-letme
mfa_arn                   = arn:aws:iam::4002019901:mfa/1pwd
session_name              = user_letme
session_duration          = 3600


[lockedinspace]
aws_source_profile        = lockedinspace
aws_source_profile_region = us-east-1
dynamodb_table            = cross-account-credentials-lockedinspace
mfa_arn                   = arn:aws:iam::5256715791:mfa/google-auth
session_name              = lockedinspace
session_duration          = 900
```

If we want to select lockedinspace context we can do so by using this command:

```bash
letme config --context lockedinspace
```

We can check our current and the full list of contexts by using this command:

```bash
letme config
```

Here's a real example:

![](../docs/letme-setup/img/letme-config-context.gif)

## Credentials process

This is a new implementation of the `obtain` command. Now, instead of executing `letme obtain` every time our credentials have been expired, we can leverage this to AWS CLI.

:::info
For more information, see: [AWS CLI configure credential process](https://docs.aws.amazon.com/cli/v1/userguide/cli-configure-sourcing-external.html)
:::

## Renew

Before, every time we executed `letme obtain` new credentials were generated (**even though they were still valid**). 

Now, if credentials are valid, **no call to the Assume Role API will be made and you'll use the same credentials**. But with the addition of `--renew` flag you can force the generation of new credentials:

```bash
~/letme
❯ letme obtain lockedinspace        
Assuming role with the following session name: 'lockedinspace' and context: 'lockedinspace'
letme: using cached credentials. Use argument --renew to obtain new credentials.
letme: use the argument --profile 'lockedinspace' to interact with the account.

~/letme
❯ letme obtain lockedinspace --renew
Assuming role with the following session name: 'lockedinspace' and context: 'lockedinspace'
Enter MFA one time pass code: 059829
letme: use the argument --profile 'lockedinspace' to interact with the account.
```