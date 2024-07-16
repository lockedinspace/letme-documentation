---
sidebar_position: 3
slug: /commands/list
---

# List

Get all avalaible accounts.

## Usage

- List all accounts.

```bash
letme list
```

- List outputing a JSON file:

```bash
letme list -o json
```

- List filtering result based on tags configured on DynamoDB Table:

```bash
letme list --filter=admin
```
