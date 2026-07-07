# Setup Guide

This guide explains how to install, configure, and run Katelyn for local development or production deployment.

---

# Prerequisites

Before setting up the project, ensure the following software is installed:

* Node.js **v20** or later
* PostgreSQL
* Git

You'll also need:

* A Telegram Bot Token obtained from **@BotFather**
* One or more Google Gemini API keys

---

# 1. Clone the Repository

```bash
git clone <repository-url>
cd Katelyn
```

---

# 2. Install Dependencies

Install all required packages.

```bash
npm install
```

---

# 3. Configure Environment Variables

Create a `.env` file in the project root.

```env
# Server Configuration
PORT=5000
BOT_URL=https://your-domain.com

# Telegram
BOT_API_KEY=your_bot_token

# Gemini API Keys
GOOGLE_API_KEY1=
GOOGLE_API_KEY2=
GOOGLE_API_KEY3=
GOOGLE_API_KEY4=
GOOGLE_API_KEY5=

# PostgreSQL
DATABASE_URL=postgresql://username:password@host:port/database
```

### Environment Variables

| Variable            | Description                                            |
| ------------------- | ------------------------------------------------------ |
| `PORT`              | HTTP server port                                       |
| `BOT_URL`           | Public URL of the deployed application                 |
| `BOT_API_KEY`       | Telegram Bot API token                                 |
| `GOOGLE_API_KEY1-5` | Gemini API credentials used for automatic key rotation |
| `DATABASE_URL`      | PostgreSQL connection string                           |

> **Note**
>
> Katelyn supports multiple Gemini API keys. During runtime the application automatically rotates between available keys to distribute requests and temporarily removes failed keys from rotation until they recover.
>
> A single key is supported, although multiple keys are recommended for improved availability.

---

# 4. Create the Database

Create an empty PostgreSQL database and update the `DATABASE_URL` to reference it.

No manual table creation is required.

---

# 5. Database Initialization

Database migrations execute automatically during application startup.

When Katelyn launches for the first time, the migration system creates any missing tables required by the application.

If automatic migration is disabled, migrations can also be executed manually.

```bash
node migrations.js
```

Always verify that migrations complete successfully before using the bot.

---

# 6. Start the Application

## Development

```bash
npm run dev
```

## Production

```bash
node index.js
```

---

# 7. Verify the Installation

A successful startup should produce the following results:

* Application starts without runtime errors.
* PostgreSQL connection is established.
* Database migrations complete successfully.
* Telegram bot connects successfully.
* HTTP server starts.
* Health monitoring initializes.
* The bot responds to `/start`.

If all of the above succeed, the installation is complete.

---

# Troubleshooting

## Bot does not respond

* Verify `BOT_API_KEY` is valid.
* Confirm the bot has been started from Telegram.
* Ensure the application is running without startup errors.

---

## Database connection failed

* Verify PostgreSQL is running.
* Confirm the `DATABASE_URL` is correct.
* Ensure the target database exists.

---

## Gemini API errors

* Verify each configured API key is valid.
* Confirm at least one key has available quota.
* Check that environment variables were loaded correctly.

---

## Missing environment variables

If the application exits during startup, verify every required variable exists inside the `.env` file (Checkout the .env.example) file.



---

## Migration failures

If migrations fail:

1. Verify database permissions.
2. Ensure the database exists.
3. Restart the application after correcting the issue.

---

# Updating

To update an existing installation:

```bash
git pull
npm install
```

Restart the application after updating.

Any new database migrations will execute automatically during startup.

---

# Additional Documentation

* **README.md** — Project overview
* **ARCHITECTURE.md** — Internal architecture and execution model
* **CHANGELOG.md** — Release history
* **LICENSE** — License information

---

# Need Help?

If you encounter an issue that isn't covered here, open an issue in the repository or contact the project maintainer.
