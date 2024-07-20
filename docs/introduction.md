---
sidebar_position: 1
slug: /
---

# Introduction

Let's find out **the What** and **Why** of letme.

![banner](/img/letme-banner.webp)


## What it is

<p align="center"><b>letme is a reliable, secure and fast way to switch between AWS accounts from the CLI written in Go.</b></p>

:::info

It does so by creating or updating your AWS credentials under your AWS files and using profiles.
:::



## What it's not
This software is **NOT** intended for:

- Securing your AWS files, **letme just reads and writes to them**.
- **You are responsable** to prevent unauthorized access to those files.
- **Securing your AWS infrastructure** (requiring MFA in your trust relationships, using a role with fine-grained permissions, ...)


## Why

As current AWS administrators, we've found that **it wasn't easy to manage** credentials from multiple accounts and **follow AWS best practices** at the same time. To solve all this issues and more we created
`letme`. A tool born from the scratch with the following goals and design principles:

- **Reduce the hassle** of interacting with AWS services on **multiple accounts**.
- **Leverage** the interaction with AWS Assume Role API and AWS config files.
- **Do not tinker** with end-user machine:
    - ~~Using the keychain~~.
    - ~~Updating environment variables~~.
    - ~~Executing other programs~~.
    - ~~etc.~~
- **No more**: _"From my local computer works."_

:::tip 

letme is compatible with **Linux**, **MacOS** and **Windows**.

:::
