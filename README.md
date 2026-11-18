# GM League Season 4 — Event Management System

A Flask web app for running a university sports league: player registration,
franchise (team owner) registration, a live player auction (bidding), an admin
console, Excel exports, and automated confirmation emails over SMTP. A separate
`scorecard` blueprint provides live match scoring, standings and stats.

## Features

- **Registration** – players (per sport) and team owners, with photo/logo uploads.
- **Auction** – per-sport, per-gender bidding with team budgets.
- **Admin console** – dashboard, edit/delete/bulk-delete records, reset bids,
  resend confirmation emails, SMTP test.
- **Exports** – per-sport and all-sport summaries and owner lists as `.xlsx`.
- **Scorecard** – scorers' console, match pages and standings (`/scorecard`).

## Tech stack

Python, Flask, Flask-SQLAlchemy, Flask-Migrate, XlsxWriter, python-dotenv.
SQLite locally; PostgreSQL (e.g. Supabase) in production. Deploy configs for
Vercel (`vercel.json`) and Netlify (`netlify.toml`) are included.

## Getting started

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # or create .env with the variables below
python app.py          # http://127.0.0.1:5000
```

## Configuration

Settings are read from environment variables (or a `.env` file). Never commit
secrets.

| Variable | Purpose | Default |
| --- | --- | --- |
| `SECRET_KEY` | Session/CSRF signing key. **Set this in production.** | random per start |
| `DATABASE_URL` | SQLAlchemy URL | `sqlite:///league.db` |
| `ADMIN_USERNAME`, `ADMIN_PASSWORD` / `ADMIN_PASSWORD_HASH` | Admin login | – |
| `MAIL_ENABLED`, `MAIL_SUPPRESS_SEND` | Toggle email / dry-run | `true` / `false` |
| `MAIL_SERVER`, `MAIL_PORT`, `MAIL_USE_TLS`, `MAIL_USE_SSL` | SMTP server | `smtp.gmail.com`, `587` |
| `MAIL_USERNAME`, `MAIL_PASSWORD`, `MAIL_SENDER_NAME`, `MAIL_SENDER_EMAIL`, `MAIL_REPLY_TO`, `MAIL_ADMIN_BCC` | SMTP credentials and sender | – |
| `LEAGUE_NAME`, `LEAGUE_SEASON`, `LEAGUE_UNIVERSITY`, … | Branding text | see `app.py` |
| `DEFAULT_TEAM_BUDGET` | Starting auction budget per team | see `app.py` |
| `MAX_UPLOAD_MB` | Upload size limit | `8` |
| `SESSION_COOKIE_SECURE` | Secure cookies (forced on when serverless) | `false` |
| `FLASK_DEBUG`, `HOST`, `PORT` | Local dev server | `true`, `127.0.0.1`, `5000` |

On serverless hosts (Vercel / AWS Lambda) debug mode is forced off and the
Supabase pooler (IPv4) connection string should be used for `DATABASE_URL`.

## Project layout

```
app.py          Main Flask app: routes, models, config
mailer.py       SMTP email automation
scorecard/      Live scoring blueprint (admin, scorer, public, stats)
templates/      Jinja templates (incl. emails/)
static/         CSS, images and uploaded media
netlify/        Netlify function entry point
```
