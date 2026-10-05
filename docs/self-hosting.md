# Self-host Kortix on your own box, VPC or on-prem

Kortix runs as one Docker Compose stack, and a self-hosted instance is the same images the managed cloud runs. This guide covers the hardware, the commands, model keys, updates and backups. The detailed operator reference lives at the [Kortix self-hosting docs](https://kortix.com/docs/host), and the deployment overview is on the [self-hosted page](https://kortix.com/self-hosted).

## What you need

- A Linux box. The one-shot bootstrap script is Linux only; on another OS install the CLI and use the manual path below.
- Docker Engine with the Compose plugin.
- 2 vCPU and 4 GB RAM as a floor, 4 vCPU and 16 GB for real use.
- Ports 80 and 443 open, with a domain. The bundled Caddy proxy uses them to issue a TLS certificate.
- An A or AAAA record for your domain and for `api.<domain>`, both pointing at the box.

Agent sessions run on a separate sandbox provider, not on this stack. The default is Daytona; Platinum and E2B are also supported.

## 1. Install the CLI

```bash
curl -fsSL https://kortix.com/install | bash
```

## 2. Point DNS, then initialize

Create the two DNS records and open ports 80 and 443 first. Then generate the instance:

```bash
kortix self-host init --domain kortix.example.com
```

The CLI asks only the things it cannot know. It generates every port, URL, password, signing key and Compose default itself, and writes the whole instance to one directory.

To evaluate with no domain, use a Cloudflare tunnel instead:

```bash
kortix self-host init --tunnel cloudflare
```

The tunnel URL changes on every restart. Use that mode for evaluation, not production.

## 3. Start the stack

```bash
kortix self-host start
```

One command brings the whole control plane up: accounts, projects, repos, secrets, connectors, policies and audit. There is no separate provisioning step and no console to click through.

## 4. Set the sandbox provider key

The stack cannot start sessions without a sandbox provider key.

```bash
kortix self-host configure
```

`configure` is an interactive prompt for the sandbox provider key and, optionally, a managed-git token. Configure GitHub later in the dashboard under Settings, then Git.

## 5. Check on it

```bash
kortix self-host status
kortix self-host logs kortix-api
kortix self-host doctor
```

Use `kortix hosts ls` to see the hosts your CLI knows. A host is one Kortix API endpoint with its own token, so `selfhost` is your own stack and `cloud` is the managed one. Switch with `kortix hosts use selfhost`, and back with `kortix hosts use cloud`.

## Point agents at any model with your own keys

A self-hosted instance has no managed model lineup. You connect the providers you already pay for, and every model call routes through the gateway running on your own box.

```bash
kortix providers set anthropic sk-ant-...
kortix providers set openai sk-...
kortix providers set openrouter sk-or-...

kortix providers login chatgpt
kortix providers ls
```

Keys are stored as an encrypted project secret and injected at session boot. Model choice is per agent, per session or per message, so you can switch providers without changing the deployment. Anthropic, OpenAI, Google, Groq, xAI, DeepSeek, Mistral, Bedrock and OpenRouter are supported, along with an OpenAI-compatible endpoint you run yourself.

## Updates

Every instance runs an in-compose updater that checks once a day at a time you set, runs any new database migration, then starts the new services before it stops the old ones. Pin an exact version instead with:

```bash
kortix self-host update --tag 0.9.84
```

Turn the updater off with `--auto-update off`. Rotate any generated value with `kortix self-host env rotate`, and inspect one with `kortix self-host env ls`.

## Backups

There is no separate backup service. Back up three paths under `~/.config/kortix/self-host/<instance>/`:

- `volumes/db/data`, the Postgres database.
- `volumes/storage`, the file storage.
- the instance `.env`, which holds every secret and signing key the instance uses.

Copy all three before a destructive command. Restoring means putting them back.

## VPC and on-prem

The same stack runs inside your own network. Your database, files, repos and policies stay on disk you control, and the data plane never calls back to us for model access. A laptop and a cloud VM run the same artifact; the domain is one environment variable and the deployment does not change.

Fully isolated topologies need the sandbox tier moved inside the network with the rest of the stack, and those are scoped with Kortix rather than self-served. Start with the standard stack and move the sandbox tier when your security model requires it.

## Licence

Kortix is open source (Elastic License 2.0) — self-host, read and modify the code. The source is at [Kortix on GitHub](https://github.com/kortix-ai/suna).

## See also

- [Set up the CLI and run your first session](kortix-setup.md).
- [Self-hosting options compared on the campaign site](https://openworkalternative.com/self-hosting.html).
- [Frequently asked questions](faq.md).
