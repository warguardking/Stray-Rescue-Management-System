# SRMS - Stray Rescue Management System

The project uses HTML/CSS/JavaScript, a Python standard-library HTTP server, and SQLite.

## Run locally
1. Install Python 3.8+.
2. Set the SMTP environment variables listed below in the terminal before running the app.
3. Run `python server.py` from this folder.
4. Open `http://localhost:8000`.

## Email verification setup (Gmail SMTP)
Use a Gmail account that you control. Turn on 2-Step Verification and create an App Password in Google Account settings. Use the 16-character app password, not your normal Gmail password.

Set these environment variables (never commit real values to Git):
- `SMTP_HOST=smtp.gmail.com`
- `SMTP_PORT=587`
- `SMTP_USER=your-sender@gmail.com`
- `SMTP_PASSWORD=your-16-character-app-password`
- `SMTP_FROM=your-sender@gmail.com`

Windows PowerShell example for the current terminal session:
```powershell
$env:SMTP_HOST="smtp.gmail.com"
$env:SMTP_PORT="587"
$env:SMTP_USER="your-sender@gmail.com"
$env:SMTP_PASSWORD="your-16-character-app-password"
$env:SMTP_FROM="your-sender@gmail.com"
python server.py
```

Registration, resend-code, and forgot-password endpoints now send the code by email. The code is no longer returned to the browser or shown on the page/terminal.

## Deployment note
The server reads Render's `PORT` environment variable. For Render, create a **Web Service** from this folder/repository, use `python server.py` as the Start Command, and add the five SMTP variables in the service's Environment settings. This app uses SQLite in a local file; Render's default filesystem is ephemeral, so database contents may be lost on redeploy. For persistent storage, attach a Render persistent disk (paid on Render) with mount path `/var/data` and set `SRMS_DATA_DIR=/var/data` in Environment; the app is configured to store `srms.db` there. Otherwise, use the free service for testing only and expect database contents to be lost after a redeploy.
