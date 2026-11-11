# Nexus Hub

**Talk to your own AI agents from your iPhone, by text or voice, from anywhere. Your chats stay on your own computer.**

Nexus Hub is an iPhone app plus a small program for your computer, **Nexus Gateway**. The gateway connects the app to AI models you choose: models running on your own computer (LM Studio, Ollama and others), online services you have a key for (Claude, OpenAI, OpenRouter, Groq), and agent servers such as Hermes Agent. Your iPhone reaches the gateway over your private Tailscale network, at home or on mobile data.

> **Beta.** Nexus Hub is in public testing. Expect rough edges, and please report what you find (see [Feedback](#feedback)).

## Download

| | Where | Notes |
|---|---|---|
| **iPhone app** | **[Join the TestFlight beta](https://testflight.apple.com/join/d1G4TSSw)** | Install Apple's free TestFlight app, then open the link on your iPhone. |
| **Windows** (10 or 11, 64-bit) | **[Releases](https://github.com/ccrunnerli/nexus-hub/releases)** › newest › `NexusGatewaySetup-<version>.exe` | Not code-signed yet; see [Windows warns about the installer](#windows-warns-about-the-installer). |
| **Mac** | Coming soon | |

## What you can do with it

- **Chat with each of your agents** on its own, with threads, search, pinning and archiving.
- **Talk by voice.** Speak, and hear the reply in a voice you pick for each agent.
- **Run a team of agents in one room.** In the Hub, mention agents with `@Name` to bring them in, or have every agent answer. They answer one after another, and each one sees the replies before it.
- **Use local and online models side by side.**
  - On your computer: LM Studio, Ollama, llama.cpp and vLLM are found automatically.
  - Online, with your own key: Anthropic (Claude), OpenAI, OpenRouter, Groq, or any OpenAI-compatible service.
  - Agent servers: Hermes Agent, keeping its own memory and tools. When it wants to run a risky command, it asks you in the chat, and you answer "approve" or "deny" from your phone.
  - Local models are found automatically. Claude, Hermes and other online services are connected once in the setup page, with no coding: **[how to connect Claude and Hermes](CONNECT-CLAUDE-AND-HERMES.md)**.
- **Send photos, text files, PDFs and Word documents.** Under each file you see which agents received it and which have read it.
- **Keep what matters.** Copy or select any part of a reply, and export a message or a whole thread as text, Markdown or PDF.
- **Get notified** when a long reply is ready, and lock the app with Face ID.
- **Try it first.** A demo mode with sample agents works without any computer or setup.

## Coming in the next gateway release

These are built and in testing. The iPhone app already has them, but they need a Nexus Gateway newer than 0.4.3, which is not published yet.

- **Long-term memory.** Agents keep short notes across conversations: facts about you, your preferences and ongoing work.
  - In an agent's chat: `/remember <note>` saves one, `/memory` lists them, `/forget 3` or `/forget all` removes them.
  - Agents can also save notes themselves. Claude does it with a real memory tool.
  - On the **Agents** tab, **Memory** shows each agent's notes, and you can add or delete them.
  - Notes are stored on your computer. They are sent only to that agent's own model, with your messages, so an online agent such as Claude receives them too.
- **Web search.**
  - Claude can search the web with Anthropic's built-in search, and lists its sources at the end of the reply.
  - Local models can search through your own [SearXNG](https://docs.searxng.org/) server, so searches stay private and free.
  - A "🔎 Searching" line shows what the agent looked up.
- **Agent profiles.** Give an agent a written identity: who it is, what it's good at, its limits, and which teammate to send work to.
- **Approve or deny with a tap.** When Hermes Agent asks permission to run a command, **Approve** and **Deny** buttons appear in the chat.
- **Know what Claude costs.** Type `/usage` in a chat to see this month's estimated Claude spending, by model, including web searches and the share used by memory. Your actual bill is in the Anthropic Console.

In this first version, memory, web search and agent profiles are switched on for each agent in the gateway's settings file. The release notes will explain how.

## How it works

```
iPhone (Nexus Hub)  ──Tailscale, HTTPS──▶  your computer (Nexus Gateway)  ──▶  your AI models
                                             chats are stored here              local, online, or agent servers
```

- There is no Nexus Hub account and no Nexus Hub server. The app talks only to the gateway on your computer.
- Your conversations are saved on that computer, and shown in the app on your iPhone.
- Messages go to an online service only when you connect one with your own key; that service's own privacy policy then applies.

## Get started

1. **On your computer:** download and run the Nexus Gateway installer. When it finishes, a setup guide opens in your browser and walks you through:
   1. **AI models:** finds LM Studio or Ollama, or connects an online service with your key (see [Connect Claude and Hermes](CONNECT-CLAUDE-AND-HERMES.md)).
   2. **Tailscale:** installs it and signs you in (free for personal use).
   3. **Options:** start Nexus Gateway when you sign in to the computer.
   4. **Pair iPhone:** shows a QR code, plus the address and token if you'd rather type them.

   The first time, turn on **MagicDNS** and **HTTPS Certificates** in the DNS page of the [Tailscale admin console](https://login.tailscale.com/admin/dns); the guide links there.
2. **On your iPhone:** install Nexus Hub from TestFlight and install **Tailscale** from the App Store, signed in to the same account.
3. **Pair:** in Nexus Hub, tap **Scan pairing code** and point it at the QR code on your computer. A connection check tells you if anything is missing.
4. **Make your first agent** from a model the app lists, or open the setup guide again later from **Settings › Setup**.

The setup page stays available at `http://127.0.0.1:8790` on the computer. Use it to pair another iPhone, make a new pairing code, or add connections.

## Windows warns about the installer

The installer is not code-signed yet, so Windows SmartScreen shows **"Windows protected your PC"**. Click **More info › Run anyway**. Some antivirus programs may also flag or quarantine unsigned builds; if yours does, you can restore the file and add an exception, or wait for a signed release.

To check that your download is the file published here, compare its SHA256 checksum with the one in the release notes. In PowerShell:

```powershell
Get-FileHash $HOME\Downloads\NexusGatewaySetup-<version>.exe
```

## Privacy

Read the [privacy policy](PRIVACY.md). In short: no accounts, no analytics, no tracking, and your chats are stored on your own computer.

## Feedback

- **In the app:** long-press any AI reply › **Report this reply**.
- **Bugs and ideas:** open an issue in this repository.
- **Email:** support@swiftserviceapp.com

This repository holds the downloads and documentation. The source code is not public.
