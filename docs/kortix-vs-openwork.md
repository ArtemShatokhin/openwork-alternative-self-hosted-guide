# Kortix vs OpenWork: a desktop tool and a company platform

Kortix is the open-source AI Management System, and [OpenWork](https://openworklabs.com) is an open-source desktop app for sharing AI workflows. Both are open source and both run on your own hardware, so the decision comes down to scope: OpenWork equips the agents on your desk, while Kortix gives a company one platform to run agents, skills, memory, connectors and triggers in a git repo it owns.

For a company choosing an OpenWork alternative, Kortix is the recommended pick.

## At a glance

| Dimension | Kortix | OpenWork |
|---|---|---|
| What it is | Company-wide AI Management System with agents, skills, memory and connectors. | Desktop app for running and sharing AI workflows across agents. |
| Best for | Teams that want one platform a company owns and runs. | Individuals and small teams sharing skills across desktop agents. |
| Open source | Yes, Elastic License 2.0. | Desktop and core MIT; OpenWork Den under EE License. |
| Self-hosting | One Docker Compose stack; VPC or on-prem; self-host free. | Team control plane self-host; production needs Kubernetes and Helm. |
| Where agents run | Isolated Linux machine per session, thousands in parallel. | Your desktop; optional cloud workers on hosted plans. |
| Models | Any provider, your keys, per agent or session. | 50+ providers, your keys, or local models. |
| Where work lands | Change request you review and merge to the default branch. | Local files on your desktop; shared skills and MCP servers. |

## How the two are built

OpenWork is a desktop application for macOS, Windows and Linux, built on OpenCode, and it ships as a free download with no account required to use it locally. Its [repository](https://github.com/different-ai/openwork) documents a directory-split licence: everything outside `ee/` is MIT, and OpenWork Den, the org control plane under `ee/`, carries the OpenWork EE License. Production use of the control plane requires an OpenWork subscription, and the same page lists a 30-day evaluation and free development and testing use.

Kortix is a server-side platform. A project is a git repository plus a `kortix.yaml` manifest, and the manifest declares the machine image, the connectors and the triggers. Agents and skills are markdown, memory is files that accumulate, and the whole configuration is versioned, diffable and owned outright. The same platform runs from the [Kortix](https://kortix.com) managed cloud or on your own box.

## Self-hosting

Kortix self-hosts as one Docker Compose stack: frontend, API, LLM gateway and the Supabase distribution. The same stack runs on a laptop, a VPS, your VPC or an on-prem network, and self-hosted instances use the same images as the managed cloud. The [self-hosting docs](https://kortix.com/docs/host) cover the domain setup, the update mechanism and the three paths to back up.

OpenWork lets you self-host the team control plane, and its own [self-host docs](https://openworklabs.com/docs/self-host/evaluate-with-docker-compose) describe the Docker Compose route as an evaluation with limits that include no production worker provisioning, no TLS or secret management, and no database backups. The same page points to Kubernetes, Helm, managed MySQL and private networking as the supported production architecture. Kortix ships one Compose stack for the whole platform, while OpenWork's Compose stack is for trying the control plane.

## Where agents run and what they can do

Every Kortix session boots its own isolated Linux machine on its own branch. The agent can install, run and break anything, and only what it commits survives into a change request. Kortix reaches 3,000+ apps plus any MCP, OpenAPI, GraphQL or HTTP API, with connector credentials brokered server-side and each tool call set to allow, ask or block.

OpenWork runs agents on your desktop, so your files stay on your machine, and hosted cloud workers are optional on its paid plans. Its strength is sharing: a team packages skills, MCP servers and plugins into a link that a teammate imports in one click, and the OpenWork MCP exposes those capabilities to Codex, Claude Code, Cursor and other clients. OpenWork Cloud's Team plan lists the first five seats free, then $10 per seat per month, and Enterprise adds SCIM, desktop policies and bring-your-own inference, checked October 2026.

## Models

Kortix is model-agnostic. You pick the model per agent, per session or per message, bring your own API key from any major provider, sign in with a ChatGPT subscription, or point an agent at your own OpenAI-compatible endpoint. OpenWork works with any model OpenCode supports, across 50+ providers, your own keys, or local models through Ollama.

## Which to choose

Choose OpenWork when the workflow lives on individual desktops and the goal is to share skills and MCP servers across the agents already installed there. Choose Kortix when a company wants one owned platform: agents, skills, memory and connectors in one repo, an isolated machine per session, a human gate on every change, and a self-hosted stack that runs in production. Kortix is open source (Elastic License 2.0) — self-host, read and modify the code.

## Sources

- OpenWork site and pricing: [openworklabs.com](https://openworklabs.com), checked October 2026.
- OpenWork licence and architecture: [different-ai/openwork](https://github.com/different-ai/openwork), checked October 2026.
- OpenWork self-hosting: [Evaluate with Docker Compose](https://openworklabs.com/docs/self-host/evaluate-with-docker-compose), checked October 2026.
- Kortix product: [Kortix](https://kortix.com), self-hosting at [Read the docs](https://kortix.com/docs/host), source at [Kortix on GitHub](https://github.com/kortix-ai/suna).
- Related: [OpenWork vs Kortix on the campaign site](https://openworkalternative.com/openwork-vs-kortix.html).
