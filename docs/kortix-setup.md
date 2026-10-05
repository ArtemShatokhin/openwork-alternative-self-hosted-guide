# Set up Kortix from a fresh terminal

This guide takes you from a fresh terminal to a merged change request. Kortix is the open-source AI Management System: you install a CLI, scaffold a project, ship it, and start a session. It is open source, so what you run is code you can read. Every command below is copy-pasteable and comes from the [Kortix quickstart](https://kortix.com/docs/quickstart).

## Before you start

Install the CLI on macOS or Linux. The installer downloads a prebuilt binary; there is no Windows binary yet.

## 1. Install the CLI

```bash
curl -fsSL https://kortix.com/install | bash
```

The installer writes the `kortix` binary and a shim on your path. Update it later with `kortix update`.

## 2. Sign in

```bash
kortix login
```

This opens your browser. After you sign in, the CLI picks your account and a default project. A user token starts with `kortix_pat_`; the CLI stores authentication per host in `~/.config/kortix/config.json`.

## 3. Create a project

A new account has no projects, so create your first one:

```bash
kortix init my-app
cd my-app
```

`kortix init` writes a project directory with the general-purpose starter. Its `kortix.yaml` declares `kortix_version: 2` and runs the OpenCode harness. To work on a project that already exists, clone it with `kortix projects clone <project-id>` instead.

## 4. Ship the project

```bash
kortix ship
```

`kortix ship` lints your `kortix.yaml`, commits local changes, pushes your branch, and prompts for any missing secret or connection. On its first run it also creates the cloud project and repo. Run it again whenever you want local changes on the project.

## 5. Start a session

```bash
kortix sessions new --prompt "Build the login page" --wait
```

The session runs in its own sandbox, on its own branch, so your project is not touched yet. `--wait` blocks until the session is ready. Every session gets its own isolated Linux machine.

## 6. Attach and steer

```bash
kortix connect
```

With no session id, `kortix connect` opens a session picker and lands you in the OpenCode TUI attached to that session. Pass an id to skip the picker: `kortix connect <session-id>`. For a lighter line-based chat, run `kortix sessions chat`. To open a raw shell in the sandbox with no agent involved, run `kortix sessions shell`.

## 7. Review and merge the change request

When the agent finishes, it opens a change request with a summary and the exact diff. Nothing reaches your default branch until you merge.

```bash
kortix cr ls
kortix cr diff 1
kortix cr merge 1
```

`kortix cr ls` lists the open change requests. `kortix cr diff <cr>` shows the unified patch. `kortix cr merge <cr>` merges it into the project's default branch.

## What you have now

You are running the same loop a team runs all day: describe the intent, the agent works in its own sandbox on its own branch, and you review the result as a diff before it lands. The CLI manages the `opencode` binary for you, so the terminal client and the session server never skew.

## Next steps

- Run Kortix on your own hardware: [self-hosting.md](self-hosting.md).
- Compare the platform with a desktop workflow tool: [kortix-vs-openwork.md](kortix-vs-openwork.md).
- Read every stable command and flag in the [Kortix CLI reference](https://kortix.com/docs/cli).
- Browse the source: [Kortix on GitHub](https://github.com/kortix-ai/suna).
