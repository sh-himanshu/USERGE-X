# USERGE-X

A powerful, pluggable Telegram UserBot built with Python and [Pyrogram](https://github.com/pyrogram/pyrogram).

> **Note:** This is an archived project from my early coding days. It is no longer actively maintained.

---

## Overview

USERGE-X is an extensible Telegram UserBot that provides a modular plugin architecture for automating tasks, managing groups, and adding custom functionality to a Telegram account. It supports both **user-mode** and **bot-mode** (dual-mode) operation, with a built-in plugin system that makes it straightforward to extend.

## Tech Stack

| Category | Technology |
|---|---|
| Language | Python 3.8+ |
| Telegram MTProto | Pyrogram (custom fork) |
| Database | MongoDB (via Motor — async driver) |
| Cloud Storage | Google Drive API |
| Deployment | Docker, Heroku |
| Shell Scripting | Bash (init, bootstrap, logging) |

## Architecture

```
userge/
├── core/               # Client, database, raw Pyrogram extensions
│   ├── ext/            # Connection pooling, raw client wrapper
│   ├── methods/        # Chat, decorator, message, user, and utility methods
│   └── types/          # Bound messages, filters, commands, plugin manager
├── plugins/            # Modular command plugins
│   ├── admin/          # Anti-flood, bans, locks, purge, warns
│   ├── bot/            # Alive checks, PM guard, inline helpers
│   ├── fun/            # Meme generation, stickers, carbon, quotes
│   ├── misc/           # Downloads, uploads, Google Drive, YouTube, RSS
│   ├── tools/          # Executor, system info, updater, search
│   └── utils/          # Filters, notes, translate, weather, AFK
├── utils/              # Shared helpers — aiohttp, progress bars, exceptions
└── xcache/             # Caching layer
```

## Key Features

- **Plugin System** — Hot-loadable plugins with command and filter decorators; each plugin is self-contained with its own help text.
- **Dual Mode** — Operates simultaneously as a userbot and an inline bot.
- **Group Administration** — Anti-flood, global bans/mutes, permission locks, warn system.
- **Media Tools** — Google Drive upload/download (Team Drives supported), YouTube download, Telegram file split/combine, archive handling (zip/tar/rar).
- **Utilities** — OCR, translate, weather, dictionary, currency conversion, COVID stats, speed test, web screenshots.
- **Conversation API** — Async conversation handler for interactive multi-step flows within chats.
- **Channel Logging** — Structured logging to a dedicated Telegram channel.
- **Database-Backed State** — Persistent configuration, notes, filters, and permissions via MongoDB.

## Plugin Example

```python
from userge import userge, Message, filters

LOG = userge.getLogger(__name__)
CHANNEL = userge.getCLogger(__name__)

@userge.on_cmd("test", about="Run a quick diagnostic")
async def test_cmd(message: Message):
    LOG.info("starting test command...")
    await message.edit("testing...", del_in=5)
    await CHANNEL.log("testing completed!")

@userge.on_filters(filters.me & filters.private)
async def echo_filter(message: Message):
    await message.reply(f"you typed - {message.text}", del_in=5)
    await CHANNEL.log("filter executed!")
```

## Getting Started

### Prerequisites

- Python 3.8+
- [Telegram API credentials](https://my.telegram.org/apps)
- [MongoDB instance](https://cloud.mongodb.com/)
- *(Optional)* [Google Drive API credentials](https://console.developers.google.com/)

### Local Setup

```bash
# Clone and enter the project
git clone https://github.com/sh-himanshu/USERGE-X.git
cd USERGE-X

# Create a virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp config.env.sample config.env
# Edit config.env with your API_ID, API_HASH, DATABASE_URL, etc.

# Generate a string session
bash genStr

# Run
bash run
```

### Docker

```bash
docker-compose -f resources/docker-compose.yml up --build
```

## Credits

- [Pyrogram](https://github.com/pyrogram/pyrogram) — Telegram MTProto API framework
- [Motor](https://github.com/mongodb/motor) — Async MongoDB driver
- [Google API Python Client](https://github.com/googleapis/google-api-python-client)
