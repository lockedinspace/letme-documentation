---
sidebar_position: 4
---

# Obtain

Used to acquire credentials from the given account by modifying `~/.aws/config` and `~/.aws/credentials`.

## Usage

- Get credentials:

```bash
letme obtain ${ACCOUNT_NAME}
```
:::info
If MFA is configured it will ask for the token.
:::

- Get credentials with inline mfa:

```bash
letme obtain ${ACCOUNT_NAME} --inline-mfa ${TOKEN}
```

- Configure credentials process:

```bash
letme obtain ${ACCOUNT_NAME} --credential-process
```

:::danger
Flag `--credential-process` **does not work with MFA configured.**
:::

### Assume Role with MFA

![](./img/letme-obtain-mfa.gif)

### Multi-Account assume role chaining

![](./img/letme-obtain-chained-mfa.gif)