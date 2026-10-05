---
name: self-host-check
description: "Check a running self-hosted Kortix deployment: stack health, sandbox provider, model providers, updates and backups. Use after a ship, on a schedule, or when a session will not start."
---

# Self-host check

Run this skill against a box where the Kortix stack is installed. Kortix is open source, and a self-hosted instance runs the same images as the managed cloud. It is the AI Management System a company runs on its own hardware, and this skill reports facts; it does not restart or reconfigure anything.

## When to run it

- After `kortix ship` brings a new revision live.
- On a schedule, so a broken stack is caught before a session fails.
- When a session will not start or an agent cannot reach a model.

## Checks

### 1. Stack status

```bash
kortix self-host status
```

Pass when the services report healthy. The stack is one Docker Compose project: frontend, API, LLM gateway and the Supabase distribution. If a service is unhealthy, read its log next:

```bash
kortix self-host logs kortix-api
```

### 2. The doctor

```bash
kortix self-host doctor
```

The doctor checks the instance directory, the generated configuration and the ports. Report anything it flags before moving on.

### 3. Sandbox provider

```bash
kortix self-host env ls
```

The stack needs a sandbox provider key to start sessions; the default is Daytona, and Platinum and E2B are supported. A missing key is the most common reason a session fails to boot.

### 4. Model providers

```bash
kortix providers ls
```

The instance should list at least one provider you configured, for example `anthropic`, `openai` or `openrouter`. Model calls route through the gateway inside your own stack, so a self-hosted instance uses your keys.

### 5. Docker resource use

```bash
docker stats --no-stream
```

Compare against the memory limit on the API container. On an 8 GiB host, keep the default 640 MiB limit; on a 16 GiB host you can raise it:

```bash
kortix self-host env set KORTIX_API_MEMORY_LIMIT=1024m
```

### 6. Host and CLI

```bash
kortix hosts ls
kortix whoami
```

Confirm the CLI points at the instance you think it does. A host is one Kortix API endpoint; `selfhost` is your own stack and `cloud` is the managed one.

### 7. Updates and backups

The in-compose updater checks once a day. Confirm the policy, and confirm the three backup paths exist: the Postgres data directory, the storage directory and the `.env` that holds the instance keys.

```bash
kortix self-host status
```

## Report

Write a short health summary with the command, its result and the smallest next action for each failure. Open a change request for any fix; do not restart the stack yourself.
