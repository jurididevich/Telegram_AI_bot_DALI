# Telegram AI Bot with n8n

An AI-powered Telegram bot built with the **n8n** workflow automation platform. It features role-based access control, persistent conversation memory, and integrated DALL-E 3 image generation capabilities.

![n8n](https://img.shields.io/badge/Platform-n8n-red)
![OpenAI](https://img.shields.io/badge/AI-GPT--4--Turbo-blue)

---

## 🌟 Features

*   **Intelligent Conversations:** Powered by OpenAI GPT-4 with conversation memory.
*   **Role-Based Access:** Different AI behavior for administrators and regular users.
*   **Image Generation:** Integrated DALL-E 3 for creating images on demand.
*   **HTML Formatting:** Rich message formatting with Telegram-supported HTML tags.
*   **Conversation Memory:** Maintains context across multiple messages using buffer window memory.

## 📋 Prerequisites

*   **n8n instance** (self-hosted or cloud).
*   **OpenAI API key** (with access to GPT-4 and DALL-E 3).
*   **Telegram Bot Token** (obtain from [@BotFather](https://t.me/BotFather)).

## 🚀 Installation

### 1. Import Workflow to n8n
1.  Download the `Telegram_AI_bot_CLEANED.json` file from this repository.
2.  Open your n8n instance.
3.  Go to **Workflows** → **Import from File**.
4.  Select the downloaded JSON file.
5.  Choose **"Import as new workflow"**.

### 2. Configure Credentials
After import, you'll need to set up the following credentials:

**OpenAI API:**
1.  Go to **Credentials** in n8n.
2.  Create new **OpenAI** credential.
3.  Enter your OpenAI API key.

**Telegram Bot:**
1.  Create a bot via [@BotFather](https://t.me/BotFather).
2.  Copy the bot token.
3.  In n8n, create new **Telegram** credential.
4.  Paste your bot token.

### 3. Set Administrator ID
To enable admin detection:
1.  Open the workflow in n8n.
2.  Find the **Code** node right after the Telegram trigger.
3.  Replace `123456789` in the `leader_ids` array with your actual Telegram user ID.
    *   *To find your ID, message [@userinfobot](https://t.me/userinfobot).*

```js
leader_ids: [
  YOUR_TELEGRAM_ID
]
```

### 4. Activate Workflow
*   Click the **Active** toggle in the top-right corner.
*   The bot is now live and ready to receive messages!

---

## 🏗 Architecture

### Workflow Components
*   **Trigger Node:** Listen for incoming events via Telegram webhook listener.
*   **Authentication Flow:**
    *   *Code:* Defines administrator IDs (`leader_ids`).
    *   *Check Leader:* Compares the sender's Telegram ID with `leader_ids` and sets `isLeader`.
*   **AI Agent:** A single agent. Its prompt receives "You are a leader" / "You are not a leader" based on `isLeader`.
*   **Language Model:** OpenAI Chat Model (GPT-4-1106-preview) with configurable temperature (0.7) and frequency penalty (0.2).
*   **Memory:** Window Buffer Memory - Maintains the last 10 messages per chat session.
*   **Tools:** Dall-E 3 Tool - Enables image generation via natural language requests.
*   **Response Handlers:**
    *   *Telegram Node:* Sends AI responses back to users.
    *   *Correct Errors:* Fallback handler for HTML formatting issues.

### Image Generation Workflow
1.  AI agent detects image request intent.
2.  Triggers "Execute Workflow Trigger".
3.  Generate image in Dall-E 3 (API call to OpenAI).
4.  Delivers generated image back to the user via Telegram.

---

## 💡 Usage

### Basic Conversation
Simply message your bot in Telegram. The AI will respond contextually based on your conversation history.

### Image Generation
Ask the bot to draw or create images:
*   *"Draw a sunset over mountains"*
*   *"Create an image of a futuristic city"*
*   *"Generate a picture of a cat wearing a hat"*

### Admin Commands
The workflow tells the AI agent whether the sender is an administrator (`isLeader`). Note that this only labels the message; it does not block regular users from any feature, including image generation.

---

## ⚙️ Configuration

### Adjusting AI Behavior
Edit these in the **OpenAI Chat Model** node parameters:
*   **Temperature (0.0 - 2.0):** 
    *   Lower = more focused and deterministic.
    *   Higher = more creative and random.
    *   *Default: 0.7*
*   **Frequency Penalty (0.0 - 2.0):** Reduces repetition in responses. *Default: 0.2*
*   **Context Window:** Number of previous messages to remember. *Default: 10 messages*

### Custom System Prompts
Modify the AI's personality by editing system messages in:
*   `AI Agent` node (system message and the `isLeader` prefix in the prompt text).

---

## 🛠 Troubleshooting

*   **Bot doesn't respond:** Check if the workflow is **Active**. Verify Telegram credentials. Check n8n execution logs.
*   **HTML formatting errors:** The `Correct errors` node handles most issues. Ensure special characters `< > &` are escaped in custom messages.
*   **Image generation fails:** Verify OpenAI API key has DALL-E 3 access and sufficient credits.

## 🚀 Customization Ideas
*   Add web search capabilities using additional tools.
*   Integrate with databases for persistent data.
*   Create custom slash commands for specific actions.
*   Add file processing capabilities.

---

## 📜 License
This project is provided as-is for educational and personal use.

## 🤝 Credits
Built with [n8n](https://n8n.io), [OpenAI GPT-4](https://openai.com), and [Telegram](https://telegram.org).
