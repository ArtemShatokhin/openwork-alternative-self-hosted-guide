# OpenWork alternative: self-host the open-source Kortix AI Management System

Kortix is the open-source AI Management System, and this repository is a working guide to self-hosting it as an OpenWork alternative. Kortix is open source, so you can read and modify the code you run.

OpenWork is a desktop app for sharing AI workflows across agents. Kortix is the platform a company runs on: agents, skills, company memory, connector configuration and triggers all live as files in one git repo you own. This repo holds the manifest, the agent, the self-host check and the commands to bring that platform up on hardware you control.

## What Kortix gives a team

Kortix gives every agent its own isolated Linux machine per session. The agent can install, run and break anything; only what it commits survives. Work reaches your default branch through a change request a person reads as a diff, so nothing lands unreviewed.

The platform wires into 3,000+ apps plus any MCP, OpenAPI, GraphQL or HTTP API. Connector credentials are brokered server-side and never enter the machine, and each tool call can be set to allow, ask or block. You pick the model per agent, per session or per message, bring your own API keys, or point an agent at your own OpenAI-compatible endpoint.

## Why self-host

The whole control plane runs as one Docker Compose stack. Accounts, projects, repos, secrets, connectors, policies and audit sit on disk you control, and the stack uses the same images as the managed cloud. The same artifact runs on a laptop, a VPS or a cloud VM; a domain is one environment variable rather than a different deployment.

Self-hosting is free, and the instance includes the updater, the CLI and the gateway. Back up the instance by copying three things: the Postgres data directory, the storage directory, and the `.env` that holds the instance keys. See the [self-hosted page on kortix.com](https://kortix.com/self-hosted) for the hardware floor and the VPC and on-prem options.

## Quickstart

Install the CLI, scaffold a project, then ship it:

```bash
curl -fsSL https://kortix.com/install | bash

kortix init my-app
cd my-app
kortix ship
```

`kortix ship` creates the project on its first run, then pushes your branch. Start a session with a prompt:

```bash
kortix sessions new --prompt "Summarize this week's commits and open a change request" --wait
kortix cr ls
```

Full setup, session attachment and change-request review are in [docs/kortix-setup.md](docs/kortix-setup.md). The full command surface is in the [Kortix CLI reference](https://kortix.com/docs/cli).

## Repository layout

| Path | What it holds |
|---|---|
| `kortix.yaml` | The v2 manifest: machine image, connectors, triggers and agent governance. |
| `agents/ops.md` | The self-hosting operator agent: role, responsibilities and tool policy. |
| `skills/self-host-check.md` | A skill that checks a running self-hosted deployment. |
| `docs/kortix-setup.md` | Install the CLI, scaffold, ship and run a session. |
| `docs/self-hosting.md` | Self-host on your own box, VPC or on-prem, and point agents at your models. |
| `docs/kortix-vs-openwork.md` | A comparison of OpenWork and Kortix, sourced from each project. |
| `docs/faq.md` | Moving from OpenWork to a platform the company owns. |

## More reading

- [OpenWork vs Kortix](https://openworkalternative.com/openwork-vs-kortix.html)
- [OpenWork alternatives](https://openworkalternative.com/openwork-alternatives.html)
- [Self-hosting options](https://openworkalternative.com/self-hosting.html)
- [OpenWork for teams](https://openworkalternative.com/openwork-for-teams.html)
- [Frequently asked questions](https://openworkalternative.com/faq.html)

## Links

- Product: [Kortix](https://kortix.com)
- Documentation: [Read the docs](https://kortix.com/docs)
- Code: [Kortix on GitHub](https://github.com/kortix-ai/suna)

## Licence

Kortix is open source (Elastic License 2.0) — self-host, read and modify the code.
