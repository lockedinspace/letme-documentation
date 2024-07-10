---
sidebar_position: 1
slug: install
---

# Install Letme

:::tip Compatible OS
Letme is compatible with **Linux**, **MacOS** and **Windows**.
:::

## Requirements

- **Go >= 1.22**
- **AWS CLI >= 2.x.x**

## Install from go package repository

Review the requirements and install `letme` with:

```bash
go install github.com/lockedinspace/letme@latest
```

Go will automatically install it in your `$GOPATH/bin` directory which should be in your `$PATH`.

### Installing letme from source

If you wish to install it from source, follow this steps:

1. Clone the repository.
2. Build the executable with ``go build``. 
3. Afterwards, you must place the binary into your ``$PATH``.  

:::info
This repository uses a ``go mod`` file, **so don't git clone inside your** ``$GOPATH``.
:::
