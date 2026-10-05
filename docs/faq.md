# Moving from OpenWork to a company-owned platform

These are the questions teams ask when they outgrow a desktop workflow tool and want one platform the company owns. Kortix is the open-source AI Management System. For the broader comparison, see [Kortix vs OpenWork](kortix-vs-openwork.md).

## Can I move my OpenWork skills and MCP servers to Kortix?

Usually yes, with a quick check. Both projects use markdown skill files and MCP, so most skills copy into `skills/<name>/SKILL.md` in your Kortix repo, and an MCP server can be declared as a connector in `kortix.yaml`. Verify each skill in a session before you retire the old workflow. The Kortix manifest reference documents how connectors and skills are declared.

## Does Kortix replace the desktop app I already use?

No. OpenWork is a desktop app and Kortix is a server-side platform, so they can run side by side. Many teams keep OpenWork on individual desktops for local work and use Kortix for the jobs that need a shared repo, an isolated machine per session, and a review gate. Start with one job and move more over as the platform earns trust.

## What does it cost?

Self-hosting Kortix is free. You provide the box and your own model keys, and every model call routes through the gateway inside your stack. Managed cloud pricing is published on the [Kortix pricing page](https://kortix.com/pricing). The self-host path is the one to choose when data residency and cost control matter, since you keep the database, files and secrets on your own disk.

## Can I self-host Kortix on the same box I use for OpenWork?

Yes, if the box is Linux with Docker and Compose. Kortix needs a domain and ports 80 and 443, plus 2 vCPU and 4 GB RAM as a floor. For real use, give it 4 vCPU and 16 GB. The same stack runs on a laptop, a VPS, your VPC or an on-prem network, and the [self-hosting guide](self-hosting.md) walks through it.

## Do I have to use one model?

No. Kortix is model-agnostic, and you choose the model per agent, per session or per message. Connect Anthropic, OpenAI, Google, Groq, xAI, DeepSeek, Mistral, Bedrock or OpenRouter with your own keys, sign in with a ChatGPT subscription, or point an agent at your own OpenAI-compatible endpoint. You can switch providers without changing the deployment.

## How do agents get reviewed before changes land?

Every Kortix session works on its own branch, and work reaches your default branch through a change request a person reads as a diff. Nothing merges until you merge it, and merge is default-deny for agents. Tool calls add a second gate: each call can be set to allow, ask or block, down to the arguments, and an ask holds the call until someone approves it.

## Is Kortix open source?

Kortix is open source (Elastic License 2.0) — self-host, read and modify the code. The source is at [Kortix on GitHub](https://github.com/kortix-ai/suna).

## See also

- [OpenWork for teams on the campaign site](https://openworkalternative.com/openwork-for-teams.html)
- [OpenWork alternatives](https://openworkalternative.com/openwork-alternatives.html)
- [The campaign FAQ](https://openworkalternative.com/faq.html)
- [Set up Kortix from a fresh terminal](kortix-setup.md)
