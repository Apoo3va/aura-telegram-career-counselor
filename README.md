# Aura - Telegram Career Counselor Bot (n8n)

An open-source AI career guidance counselor bot built with [n8n](https://n8n.io) and LangChain. It integrates directly with Telegram and maintains per-user chat history so conversations feel continuous across sessions.

![Aura Profile Picture](./aura_profile_picture.png)

## What it does

Most basic Telegram AI bots treat every incoming message as a brand new conversation. This project uses n8n's LangChain integration to map each user's unique Telegram `chat.id` to a dedicated memory buffer. 

When someone messages the bot asking for resume review, career transition advice, or mock interview questions, the AI remembers their name, experience level, and industry background from previous turns.

## Prerequisites

* An active [n8n](https://n8n.io) instance (self-hosted or cloud)
* A Telegram Bot Token (from [@BotFather](https://t.me/botfather))
* An API key for an LLM provider (defaults to [Groq](https://console.groq.com) for fast, free Llama 3 inference, but easily swappable with OpenAI or Anthropic)

## Quick Start

### 1. Create the Telegram Bot
1. Open Telegram and message `@BotFather`.
2. Run `/newbot` and follow the prompts to get your HTTP API Token.
3. Optional: Run `/setuserpic` to upload `aura_profile_picture.png` as the bot's avatar.

### 2. Import the Workflow
1. Open your n8n dashboard and go to **Workflows** -> **Add Workflow**.
2. Click the top-right menu (`...`) and select **Import from File**.
3. Upload `career_guidance_workflow.json` from this repository.

### 3. Connect Credentials
In the imported workflow canvas:
* **Telegram Trigger & Send Reply nodes:** Click **Credential for Telegram API** -> **Create New** and paste your BotFather token.
* **Groq Chat Model node:** Add your Groq API key. If you prefer OpenAI (`gpt-4o`) or Claude, simply replace this node with the corresponding n8n Chat Model node and connect it to the Agent's `ai_languageModel` input.

### 4. Activate
Toggle the workflow status to **Active** in the top right corner of n8n. Open your bot on Telegram and send `/start` to begin testing.

 ## Workflow Screenshot

![Workflow Screenshot](./workflow/Workflow%20screenshot.jpeg)

## How Memory Works

The workflow contains a **Format Chat Input** node right after the Telegram trigger. This extracts `message.chat.id` and assigns it to a top-level `sessionId` variable. 

The **Window Buffer Memory** node automatically keys its conversation history off this `sessionId`. Each user gets an independent rolling window of the last 20 messages.

## Customizing the Persona

To adjust how the counselor responds, open the **AI Career Counselor** node and edit the `systemMessage` field under parameters. You can tune the prompt for specific industries (e.g., tech, healthcare, finance) or tweak the advice style (strict mock interviewer vs. supportive mentor).

## Author
Apoorva Yadav
## License

MIT : free to use, modify, and distribute.
