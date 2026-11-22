# self hosted AI agent VPS: do you need a GPU, how much RAM, and what will it really cost per month? (runtime memory table, every Ava Hosting plan, and the renewal traps)

You don't need a GPU. When the model runs at Anthropic, OpenAI or any other provider and your server only hosts the agent loop, a 2 vCPU / 4 GB machine covers Hermes Agent, OpenClaw, n8n and Claude Code. Whether that ends up cheaper than a €20–30/month AI subscription comes down to one number: how many hours a day the agent actually works.

What follows is the resource math per runtime, every plan Ava Hosting currently lists with prices, and the contract details that change the monthly figure after the promotional period.

> **Quick pick**
> - **One or two agents, API models, no browser automation:** self-hosted **Basic** — 2 vCPU, 4 GB RAM, 50 GB NVMe, **€10.00/month**.
> - **OpenClaw driving a browser, or 3–5 agents on one box:** **Pro** — 4 vCPU, 8 GB RAM, 100 GB NVMe, **€20.00/month**.
> - **A single small agent on a tight budget:** **Mini** — 1 vCPU, 2 GB RAM, 25 GB NVMe, **€5.00/month**.
>
> 👉 [See the Basic 2 vCPU / 4 GB plan at €10 per month](https://my.ava.hosting/aff.php?aff=15562)

## Every Ava Hosting plan, with current prices

Ava Hosting lists two overlapping ranges on its own ordering pages. The self-hosted range starts at €2.00/month; the VPS/VDS range starts at €11.00/year and runs to €40.00/month. All plans include full root access, a control panel, NVMe storage, unlimited transfer, DDoS protection and 24/7 support, on Intel Xeon E5 hardware with a 1 Gbps port (the Nano is the exception at 100 Mbps).

**Self-hosted range**

| Plan | vCPU | RAM | NVMe | Price | Order |
| --- | --- | --- | --- | --- | --- |
| Start | 1 | 1 GB | 15 GB | €2.00/mo | [Order Start](https://my.ava.hosting/aff.php?aff=15562) |
| Mini | 1 | 2 GB | 25 GB | €5.00/mo | [Order Mini](https://my.ava.hosting/aff.php?aff=15562) |
| Basic | 2 | 4 GB | 50 GB | €10.00/mo | [Order Basic](https://my.ava.hosting/aff.php?aff=15562) |
| Pro | 4 | 8 GB | 100 GB | €20.00/mo | [Order Pro](https://my.ava.hosting/aff.php?aff=15562) |
| Business | 6 | 12 GB | 150 GB | €30.00/mo | [Order Business](https://my.ava.hosting/aff.php?aff=15562) |
| Power | 8 | 16 GB | 200 GB | €40.00/mo | [Order Power](https://my.ava.hosting/aff.php?aff=15562) |

**VPS/VDS range**

| Plan | vCPU / RAM | NVMe | Port | Price | Order |
| --- | --- | --- | --- | --- | --- |
| VPS Nano | 1 / 512 MB ECC | 20 GB | 100 Mbps | €11.00/yr | [Order VPS Nano](https://my.ava.hosting/aff.php?aff=15562) |
| VPS One | 1 / 2 GB ECC | 25 GB | 1 Gbps | €5.00/mo | [Order VPS One](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Basic | 2 / 4 GB ECC | 50 GB | 1 Gbps | €10.00/mo | [Order VPS Basic](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Lucky | 2 / 6 GB ECC | 70 GB | 1 Gbps | €15.00/mo | [Order VPS Lucky](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Power | 4 / 8 GB ECC | 90 GB | 1 Gbps | €20.00/mo | [Order VPS Power](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Extra | 6 / 12 GB ECC | 120 GB | 1 Gbps | €30.00/mo | [Order VPS Extra](https://my.ava.hosting/aff.php?aff=15562) |
| VPS Ultra | 8 / 16 GB ECC | 160 GB | 1 Gbps | €40.00/mo | [Order VPS Ultra](https://my.ava.hosting/aff.php?aff=15562) |

Two differences decide between the ranges. The VPS/VDS listings state ECC memory, 1 Gbps ports and weekly backups included; the self-hosted listings show resources and transfer only, so confirm what backup cover comes with the plan you pick. Above €20, the self-hosted range gives more disk for the same money: 200 GB against 160 GB at the €40 tier, and 150 GB against 120 GB at €30. Neither range advertises a prebuilt OpenClaw or Hermes image, so budget for an SSH install.

## Do you actually need a GPU for a self hosted AI agent?

No, provided the model computation happens at a provider. The VPS hosts the agent program that receives your instructions, calls the model over the API and executes the tools; the inference itself runs on Anthropic's, OpenAI's or Mistral's hardware, billed per token against your own key. What the server needs is RAM, a stable network and the ability to stay on.

Local inference is a different purchase. FirstHeberg's VPS range doesn't include GPUs at all. Bluehost frames the same boundary: a GPU becomes relevant only if you plan to run LLM inference on the same server. KryoHost sets its Hermes model server and Ollama images at a 4 vCPU / 16 GB minimum and marks GPU as strongly recommended for Hermes, noting that CPU inference is slow but functional for small models. Embeddings or lightweight classification can run on a few GB of CPU memory; a commercial-grade assistant cannot.

Treat vendor copy that promises "no per-token charges, the model runs on your server" on an 8 GB plan without a GPU as marketing. The arithmetic in the next table shows why 8 GB isn't a local-model machine.

## How much RAM each runtime needs

The figures below come from each project's own documentation, as quoted by FirstHeberg, plus KryoHost's minimum plan sizes.

| Runtime | RAM to plan for | Note |
| --- | --- | --- |
| Hermes Agent | 1 GB | agent runtime, API models |
| n8n | 2 GB | workflow automation |
| OpenClaw | 2 GB | chat and tool calling |
| OpenClaw with browser control | 4–8 GB | headless browser is the cost |
| Claude Code | 4 GB |  |
| Hermes model server (local weights) | 16 GB + GPU recommended | KryoHost's minimum plan |
| Ollama, small models on CPU | 16 GB | CPU inference, slow |

The gap between "Hermes Agent at 1 GB" and "Hermes at 16 GB" isn't a contradiction, it's two products with confusingly similar names. Nous Research ships Hermes as an open-weights model family (Hermes 3 is the current line, Hermes 4 is rolling out alongside it, older variants like Hermes 13B still see use). Hermes Agent is a separate product from the same group: a runtime with persistent memory, scheduling, subagents and tool use. The 1 GB number is the agent; the 16 GB number is the model weights. If you're using an API key, you're buying for the first one.

Plan RAM is not runtime RAM, either. The OS, package manager and logs take a few hundred megabytes before your agent starts, which is why KryoHost's minimum plan for OpenClaw is 2 vCPU / 4 GB against a 2 GB runtime figure. Start at 2 vCPU / 4 GB, watch actual usage, and scale up when RAM or CPU approaches its ceiling. CPU matters less than RAM because the heavy computation is remote. Storage only becomes interesting if the agent keeps a long history or a local vector index.

## The real monthly cost, and where the break-even sits

A typical personal AI subscription runs €20–30/month and doesn't change with usage. The VPS route trades that flat fee for two variable costs: the server and the tokens.

Do the arithmetic on Ava's pricing. Basic at €10/month plus API spend matches a €25 subscription at roughly €15/month of tokens; Mini at €5/month leaves about €20. FirstHeberg's own guidance is that an agent handling a moderate volume often costs less once it runs several hours a day, and that the advantage disappears if the agent sits idle most of the time, where a usage-based subscription is easier to budget.

Three things push the VPS side down. Several agents can share one server, since the only real limit is memory rather than seats or licences. You can route cheap tasks to a cheap model and hard ones to a strong one, with each key billed separately. Switching provider is a config change, not a migration.

👉 [Start with the Mini plan at €5 per month and scale up only if RAM runs out](https://my.ava.hosting/aff.php?aff=15562)

## Contract length and renewal: the numbers the plan grid doesn't show

Contabo's OpenClaw and Hermes pages list identical prices (€5.50, €7.50, €25.00, €49.00) with the same "for the first 24 months" line, but the two footnotes disagree: one calls the figure the effective monthly rate for a 24-month subscription, the other for a 12-month subscription. Neither page states what the price becomes in month 25, and cancellation notice is listed as "None (pre-paid) or 4 weeks (post-paid)" with a one-month minimum initial term. Anyone comparing on the headline number should confirm the term and the post-promotional rate on the order page.

FirstHeberg takes the opposite approach and says so directly: the same amount in month thirteen as in month one, with configuration upgrades adding only the cost of the added resources.

Ava Hosting's plan pages quote "starting from" monthly prices with no separate renewal price published alongside them, and the billing period is selected at checkout. Before paying, confirm two things in the cart: the term you're committing to, and what the plan costs in the second billing cycle. The self-hosted ordering page also carries a free-trial flag with no term stated on the page itself, so ask support how long it runs if you plan to test that way.

## What "1-click" leaves out

On a plain VPS you get the operating system of your choice and root access, nothing else. Installing the runtime is your job: SSH in, clone the repository, install dependencies, wire up the model provider key, write a systemd unit. One host puts a first deployment at roughly fifteen minutes to an hour, which matches the shape of the work rather than any particular provider.

Support boundaries are worth knowing in advance. Hosts cover the server side — network, hardware, firewall, resource allocation — and hand back anything in the agent layer. Contabo states this explicitly for its add-ons: maintenance, upgrading and troubleshooting of the open-source software are the end user's responsibility. On the network side, "unlimited" outbound traffic normally comes with a fair-use clause that reserves the right to throttle abnormal usage patterns.

Backups are the other line item. Some hosts sell snapshots as a paid option rather than including them, so a corrupted agent database is a rebuild rather than a restore. Ava's VPS/VDS plans list weekly backups as included, and its VPS Nano listing notes port 25 is closed, which matters if you want your agent to send mail directly.

## Keeping the agent alive, and keeping it contained

Run each agent as its own systemd service with `systemctl enable` so it comes back with the machine, and add `Restart=on-failure` so a crash restarts it without a full reboot. A process manager like supervisor does the same job if you prefer.

Isolate agents from each other with a dedicated system user and configuration directory each. That avoids dependency conflicts and lets you stop one agent without touching the others. Most people run between two and five agents on a mid-sized 4–8 GB box without slowdown, as long as they don't all share one local model or one large database.

Admin interfaces are usually bound to localhost for good reason. Reach them over an SSH port forward rather than exposing the port.

For webhooks, a VPS has a public IPv4, so you open the port and requests arrive without the tunnel you'd need from a home machine. Platforms that demand an HTTPS URL need a reverse proxy in front: Nginx or Caddy with a free Let's Encrypt certificate, renewing automatically. Restrict webhooks to an IP allowlist or embed a secret token in the URL so nobody else can trigger your agent.

Containers deserve one specific rule: don't mount `docker.sock` into an agent container, since that hands it root-equivalent control of the host. Keep production credentials off the box, scope Git tokens to the repositories the agent actually needs, and review its diffs before merging. Monitoring the agent's own application logs and error alerts is yours to set up; providers watch infrastructure, not business errors.

## Hermes Agent or OpenClaw?

Both run on the same server and the same plan; the choice is about workflow shape.

OpenClaw (formerly Clawdbot/Moltbot) is open source and talks to you through WhatsApp, Telegram, Discord or Slack. It browses the web, manages files, runs commands, schedules tasks, and connects to 50+ tools including GitHub, Notion and Home Assistant. Model choice covers Claude, GPT, Gemini or a local Ollama instance.

Hermes Agent is built around persistence: memory across sessions, scheduled tasks, delegation to subagents, and reusable skills distilled from repeated workflows. Providers include OpenAI, OpenRouter or a custom endpoint.

Bluehost's positioning is a fair summary: Hermes fits persistent memory, reusable skills and scheduled jobs, while OpenClaw suits prompt pipelines, tool calling and private workflow orchestration. If you have the RAM, run both.

## FAQ

**Is 4 GB of RAM enough for a self hosted AI agent?**
For one or two agents on API models, yes. OpenClaw plus headless browser automation pushes toward 4–8 GB, and a local vector index adds more.

**Do I need a GPU on the VPS?**
Only for local inference. With a provider API, the GPU work happens on their side and your server never touches it.

**How many agents can share one VPS?**
Two to five on 4–8 GB is the common range. Memory is the constraint, not licensing.

**Can the agent receive webhooks?**
Yes. Open the port on the public IPv4, and put Nginx or Caddy with a Let's Encrypt certificate in front if the platform requires HTTPS.

**Is a VPS cheaper if the agent is idle most of the day?**
No. The flat subscription wins when usage is sporadic, because you're paying for a server that idles alongside the agent.

## Which plan to buy

Match the plan to the runtime, not to the biggest number on the grid:

- **One agent, API models, no browser** → Mini at €5/month (2 GB) or Basic at €10/month (4 GB) if you want headroom for a second agent.
- **OpenClaw with browser control, or three to five agents** → Pro at €20/month for 8 GB, or VPS Lucky at €15/month if 6 GB is enough.
- **Several agents plus a local vector index** → Business at €30/month for 12 GB and 150 GB of disk.
- **Planning to run model weights locally** → none of these. You need 16 GB minimum and a GPU, which this range doesn't offer.

Two things to confirm at checkout before you pay: the billing term and the price in the second cycle, and whether the range you chose includes the weekly backup.

👉 [Check current Ava Hosting self-hosted and VPS plan pricing](https://my.ava.hosting/aff.php?aff=15562)
