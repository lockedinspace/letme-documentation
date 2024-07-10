---
sidebar_position: 2
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
