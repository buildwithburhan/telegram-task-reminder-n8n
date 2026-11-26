# Telegram Task Reminder (n8n)

A Telegram bot built with n8n that lets a busy person manage tasks by chatting normally, by text or voice note, so nothing gets forgotten.

## Demo

personal-assistant-bot

n8n-workflow-hd

Screenshot 2026-11-26 121426

## What it does

* Understands normal messages in text or voice notes (voice notes are turned into text with OpenAI).
* Adds, lists, reschedules, renames and deletes tasks, and removes tasks that are done.
* Puts every task in a priority group (P1 to P4) and gives a short reason.
* Sends a morning briefing at 8:00 AM (Asia/Karachi) with tasks for today, tomorrow and the day after.
* Sends a reminder at the exact time the user asks for.

## How it works

There are two workflows in the `Workflow` folder.

**Workflow 1 – AI Task Assistant**

1. The user sends a message to the Telegram bot.
2. The workflow checks the chat ID. Only the allowed chat ID gets through. `/start` replies with the chat ID.
3. If the message is a voice note, it is downloaded and turned into text.
4. An AI agent (OpenAI gpt-4o-mini) reads the text. It uses tools to read and change the task table, then replies in English. It remembers the last 15 messages.
5. A separate branch runs every minute. It finds reminders that are due, sends them and marks them as sent.

**Workflow 2 – Morning Briefing**

1. Every day at 8:00 AM it reads the task table.
2. It builds one list grouped from P1 to P4 for today, tomorrow and the day after.
3. It sends the list on Telegram.

**Priority groups**

* P1: urgent and important
* P2: important, not urgent
* P3: urgent, not important
* P4: neither

Tasks are saved in an n8n Data Table called `Boss Tasks`.

## Tech used

* n8n (self-hosted)
* Telegram Bot API (bot made with BotFather)
* OpenAI (gpt-4o-mini for the agent, audio transcription for voice notes)
* n8n Data Table
* JavaScript (Code nodes)

## How to run it

You need three things: an n8n instance with a public HTTPS address (Telegram needs it), a Telegram bot token, and an OpenAI API key. Never put real tokens or keys in this repo.

1. Create a Telegram bot with [@BotFather](https://t.me/BotFather) and copy the bot token.
2. In n8n, create two credentials: **Telegram API** (your bot token) and **OpenAI API** (your key).
3. In n8n, create a Data Table named `Boss Tasks` with these text columns: `taskId`, `task`, `dueDate`, `time`, `aiSuggested`, `aiReason`, `category`, `status`, `repeat`, `reminded`, `remindAt`.
4. Import both files from the `Workflow` folder (n8n menu: Import from file).
5. Open every node that uses the Data Table and select your `Boss Tasks` table (the files contain the placeholder `YOUR\\\_DATA\\\_TABLE\\\_ID`).
6. Open every Telegram node, the **OpenAI Chat Model** node and the **Transcribe Voice** node, and select your credentials.
7. Activate Workflow 1, then send `/start` to your bot. It replies with your chat ID.
8. Replace `YOUR\\\_BOSS\\\_CHAT\\\_ID` with that chat ID in three nodes: **Only Boss Allowed** and **Send Reminder** (Workflow 1), and **Send Briefing to Boss** (Workflow 2).
9. Activate Workflow 2.

The time zone is set to `Asia/Karachi` in the workflow settings and in the Code nodes. Change it if you are in another time zone.

## What I learned

* How to give an AI agent tools (read, add, update, delete rows) so it can manage a Data Table from normal chat messages.
* How to handle Telegram voice notes: download the file, then turn it into text with OpenAI.
* How to use schedule triggers and time zones for a daily briefing and timed reminders.

## Next steps

* \[Add what you plan to improve next]

## Contact

* LinkedIn: https://www.linkedin.com/in/burhanuddin-ai-automation-expert/
* X: https://x.com/burhan\_ai53

