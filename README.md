<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69cff59be8cff23c09c40228_1500x500%20(7).png" alt="ShellBot Banner" width="700" />
</p>

<p align="center">
  <img src="https://cdn.prod.website-files.com/69082c5061a39922df8ed3b6/69cff5912a3222297e1c501b_shellbot.png" alt="ShellBot" width="160" />
</p>

<h1 align="center">ShellBot</h1>

<p align="center">
  <strong>Deploy your AI agent in 30 seconds. No Docker. No CLI. No setup.</strong>
  <br/>
  <em>AI That Works For You. Secure by Default.</em>
</p>

<p align="center">
  <a href="https://shellbot.sh"><img src="https://img.shields.io/badge/Website-shellbot.sh-000000?style=flat-square&logo=safari&logoColor=white" alt="Website" /></a>
  <a href="https://github.com/jkeatn"><img src="https://img.shields.io/badge/GitHub-jkeatn-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub" /></a>
  <a href="https://github.com/vincentkoc"><img src="https://img.shields.io/badge/GitHub-vincentkoc-181717?style=flat-square&logo=github&logoColor=white" alt="GitHub" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Verified-Shell%20Corp-4CAF50?style=flat-square&logo=checkmarx&logoColor=white" alt="Verified" />
  <img src="https://img.shields.io/badge/Built%20on-OpenClaw-8B5CF6?style=flat-square" alt="OpenClaw" />
  <img src="https://img.shields.io/badge/Platform-WhatsApp-25D366?style=flat-square&logo=whatsapp&logoColor=white" alt="WhatsApp" />
  <img src="https://img.shields.io/badge/Deploy-30s-blue?style=flat-square" alt="30s Deploy" />
  <img src="https://img.shields.io/badge/Infra-Dedicated%20VM-orange?style=flat-square" alt="Dedicated VM" />
</p>

<p align="center">
  <code>EXKGb2SNA9D7umDm7kDGvMMoZDb6Tiz32K27zRKTBAGS</code>
</p>

---

## What is ShellBot?

ShellBot is a private AI agent that runs on your own dedicated machine — not a shared sandbox. It connects to WhatsApp and actually does things: sends emails, does research, manages your schedule, writes code, handles files. Set it up in minutes. No Docker. No CLI. No config files.

Built on [OpenClaw](https://github.com) — the open-source AI agent framework you can trust.

---

## How it works

```
   You (WhatsApp)          ShellBot Agent           Services
   +------------+         +---------------+        +------------+
   |            |         |               |        | Gmail      |
   | "Schedule  |-------->| Task Parser   |------->| Calendar   |
   |  a meeting |         | Planner       |        | Web Search |
   |  with Dan" |         | Executor      |        | Code Exec  |
   |            |<--------| Guardrails    |<-------| File Mgmt  |
   +------------+         +---------------+        +------------+
                                |
                          Dedicated VM
                          (not shared)
```

1. **Message** — send a task via WhatsApp, plain English
2. **Parse** — ShellBot breaks it into steps, identifies which tools to use
3. **Execute** — emails, calendar events, research, code — handled automatically
4. **Guardrails** — built-in safety checks prevent mistakes before they happen
5. **Respond** — results delivered back to WhatsApp in seconds

---

## Features

| Feature | Description |
|---------|-------------|
| **Instant deploy** | Live in 30 seconds. No Docker, no CLI, no YAML |
| **WhatsApp native** | Works where you already are |
| **Dedicated machine** | Your own VM, not a shared container |
| **Email & calendar** | Send emails, create events, manage inbox |
| **Research** | Web search, summarize articles, compile reports |
| **Code execution** | Write, run, and debug code in a sandboxed environment |
| **File management** | Create, edit, organize documents |
| **Built-in guardrails** | Safety checks that prevent destructive actions |
| **Open-source core** | Built on OpenClaw — fully auditable |

---

## Security

ShellBot is secure by default. Every agent runs on its own dedicated virtual machine — no shared resources, no cross-contamination, no multi-tenant risk.

```
Traditional AI Agents          ShellBot
+--------------------+        +--------------------+
| Shared sandbox     |        | Dedicated VM       |
| Multi-tenant       |        | Single-tenant      |
| Shared memory      |        | Isolated memory    |
| Shared network     |        | Private network    |
| Hope for the best  |        | Guardrails built in|
+--------------------+        +--------------------+
```

- **Isolated compute** — your agent runs on its own machine
- **No data sharing** — nothing leaves your VM unless you tell it to
- **Action guardrails** — destructive actions require explicit confirmation
- **Audit log** — every action is logged and reviewable
- **Open-source** — the agent framework is fully auditable

---

## Architecture

```
                          ShellBot Runtime
    +-----------------------------------------------------+
    |                                                     |
    |   Interface            Core              Services   |
    |                                                     |
    |   - WhatsApp API       - Task Parser    - Gmail     |
    |   - Message Queue      - Step Planner   - Calendar  |
    |   - Auth Layer         - Tool Router    - Web       |
    |                        - Guardrail        Search    |
    |                          Engine         - Code      |
    |                        - Memory           Sandbox   |
    |                          Store          - File      |
    |                        - Context          System    |
    |                          Manager                    |
    +-------------------+--+------------------------------+
                        |  |
                        v  v
    +-----------------------------------------------------+
    |   OpenClaw Engine     |    Infrastructure           |
    |   - Agent framework   |    - Dedicated VM per user  |
    |   - Tool registry     |    - Auto-scaling           |
    |   - Safety layer      |    - Health monitoring      |
    |   - Plugin system     |    - Zero-downtime deploys  |
    +-----------------------------------------------------+
```

---

## Quick start

```bash
# That's it. Seriously.
curl -s https://shellbot.sh/install | sh
```

You'll get a WhatsApp QR code. Scan it. Done. Your agent is live.

---

## Built on OpenClaw

ShellBot is built on [OpenClaw](https://github.com) — an open-source AI agent framework designed for trust and transparency. Every tool call, every decision, every action is auditable. The framework is MIT-licensed and community-driven.

We believe AI agents should be open by default. If you can't read the code, you can't trust the agent.

---

## Shell Corp

ShellBot is built and maintained by **Shell Corp**.

| | Role |
|---|---|
| [jkeatn](https://github.com/jkeatn) | Co-founder |
| [vincentkoc](https://github.com/vincentkoc) | Co-founder, Engineering |

<a href="https://shellbot.sh">shellbot.sh</a>

---

<p align="center">
  <img src="https://img.shields.io/badge/Shell%20Corp-Verified-4CAF50?style=for-the-badge&logo=checkmarx&logoColor=white" alt="Shell Corp Verified" />
</p>

<p align="center">
  <sub>ShellBot by Shell Corp. Built on OpenClaw. AI that works for you.</sub>
</p>
