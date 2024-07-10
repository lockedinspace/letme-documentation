---
sidebar_position: 6
---

import useBaseUrl from '@docusaurus/useBaseUrl';
import ThemedImage from '@theme/ThemedImage';


# How it works

On this section it will be explained the **different workflows** used to **gain access** to external accounts through AWS IAM Roles. The following diagrams shows how this works:

<p align="center">
<ThemedImage
  alt="Docusaurus themed image"
  sources={{
    light: useBaseUrl('/img/letme-workflow-light.svg'),
    dark: useBaseUrl('/img/letme-workflow-dark.svg'),
  }}
/>
</p>

## Assume Role

### DynamoDB Item

Here's the **DynamoDB item** of this example:

```json title="docs/aws/templates/assume-role-item.json"
{
    "name": "lockedinspace",
    "region": [
     "eu-west-1"
    ],
    "role": [
     "arn:aws:iam::123456789012:role/simple-role"
    ]
}
```


### Process

1. User wants to obtain **lockedinspace** credentials. To do so, executes the following:

```bash
letme obtain lockedinspace
```

:::info
**See:** [letme obtain command](./letme-usage/obtain.md)
:::

2. This will execute a **GetItem** operation based on the **name**, in this case **lockedinspace**.

3. If the account exists, it will return a DynamoDB JSON.

4. After parsing the data, it will use it to execute an **AssumeRole** operation.

5. It will create/update `~/.aws/config` and `~/.aws/credentials` files based on the **new temporary keys and token**.

6. Finally, the user can access AWS Services by appending `--profile` at the end of their CLI command:

```bash
aws eks update-kubeconfig --name eks-lockedinspace-pro --profile lockedinspace
```

## Multi-account assume role chaining

### DynamoDB Item

Here's the **DynamoDB item** of this example:

```json title="assume-role-chained-item.json"
{
    "name": "lockedinspace_chained",
    "region": [
     "eu-west-1"
    ],
    "role": [
     "arn:aws:iam::123456789012:role/simple-role",
     "arn:aws:iam::098765432112:role/simple-role-second-account",
     "arn:aws:iam::456789987652:role/simple-role-third-account"
    ]
}
```

### Process

1. User wants to obtain **lockedinspace_chained** credentials. To do so, executes the following:

```bash
letme obtain lockedinspace_chained
```

:::info
**See:** [letme obtain command](./letme-usage/obtain.md)
:::

2. This will execute a **GetItem** operation based on the **name**, in this case **lockedinspace_chained**.

3. If the account exists, it will return a DynamoDB JSON.

4. After parsing the data, it will use it to execute an **AssumeRole** operation.

5. Then, it will assume try to assume `arn:aws:iam::098765432112:role/simple-role-second-account` role using the generated credentials from the previous step.

5. Afterwards, it will assume try to assume `arn:aws:iam::456789987652:role/simple-role-third-account` role using the generated credentials from the previous step.

5. It will create/update `~/.aws/config` and `~/.aws/credentials` files based on the **new temporary keys and token** obtained on the previous step.

6. Finally, the user can access AWS Services by appending `--profile` at the end of their CLI command:

```bash
aws eks update-kubeconfig --name eks-lockedinspace-pro --profile lockedinspace_chained
```