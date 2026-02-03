# Exam Result Auto-Notifier (Cron + Email)

This project is a **lightweight, reliable Python-based monitoring tool** that periodically checks an exam results webpage and **sends instant email notifications** as soon as a specific result appears.

Originally built for **NEET PG Round 3 Result (MCC website)**, the same system can be reused for **any exam, counselling round, or result portal** by changing:

* The target URL
* The keywords to watch for

---

## 🚀 Use Case

High-stakes exam results (NEET PG, NEET UG, INI-CET, JEE, UPSC, counselling rounds, seat allotments) often:

* Go live **without notice**
* Become **extremely slow or overloaded**
* Appear as a **new PDF link** rather than a new page

This tool:

* Automatically checks the official site every *N minutes*
* Detects when the **desired result link appears**
* Sends **email alerts immediately**
* Prevents duplicate notifications

You don’t need to manually refresh the website or worry about missing the announcement.

---

## 🧠 How It Works (High-Level)

1. Cron runs the script every X minutes
2. Script downloads the HTML of the target page
3. It scans table rows for specific keywords
4. When a **new matching PDF link** is found:

   * An email is sent to configured recipients
   * The link is saved locally to avoid repeat alerts
5. Subsequent runs ignore already-notified links

---

## 📁 Project Structure

```
result_cron/
│
├── neetpg_round3_checker.py   # Main monitoring script
├── neetpg_round3_seen.txt    # Stores already-notified links
├── neetpg.log                # Cron execution logs
├── venv/                     # Python virtual environment
└── README.md                 # This file
```

---

## 🧰 Prerequisites

* macOS / Linux
* Python **3.9+** (Homebrew Python supported)
* Internet connection
* Gmail account with App Password enabled

---

## 🐍 Python Environment Setup (macOS Safe Way)

Because Homebrew Python is system-managed, **always use a virtual environment**.

```bash
cd ~/Downloads/result_cron
python3 -m venv venv
source venv/bin/activate
pip install requests beautifulsoup4
```

Verify:

```bash
python --version
pip list
```

---

## ⚙️ Configuration (Most Important Section)

Open `neetpg_round3_checker.py` and update the **CONFIG section**.

### 1️⃣ Target URL

```python
URL = "https://mcc.nic.in/current-events-pg/"
```

Replace this with:

* Exam results page
* Counselling updates page
* Notices / announcements page

---

### 2️⃣ Keywords to Detect Result

```python
KEYWORDS = ["final result", "round 3", "pg counselling"]
```

The script triggers **only if all keywords appear in the same row**.

Examples:

| Exam            | Keywords                        |
| --------------- | ------------------------------- |
| NEET UG Result  | `['final result', 'neet ug']`   |
| INI-CET         | `['result', 'ini-cet']`         |
| JEE Counselling | `['seat allotment', 'round 2']` |
| UPSC Final      | `['final result', 'upsc']`      |

---

### 3️⃣ Email Configuration (Multiple Recipients Supported)

```python
EMAIL_FROM = "yourgmail@gmail.com"
EMAIL_TO = [
    "recipient1@gmail.com",
    "recipient2@gmail.com"
]
EMAIL_PASSWORD = "YOUR_GMAIL_APP_PASSWORD"
```

⚠️ **Do NOT use your normal Gmail password**.

---

## 🔐 Gmail App Password Setup

1. Enable **2-Step Verification** in Google Account
2. Go to **App Passwords**
3. Generate password for *Mail → Mac*
4. Paste the 16-character password into the script

This is required for automated email sending.

---

## 🧪 Manual Test (Recommended)

Run once manually:

```bash
python neetpg_round3_checker.py
```

Expected output (before result):

```
No Round 3 final result yet
```

If result is already out, email will be sent immediately.

---

## ⏰ Cron Job Setup (Every 10 Minutes)

Find Python path inside venv:

```bash
which python
```

Example:

```
/Users/username/Downloads/result_cron/venv/bin/python
```

Edit crontab:

```bash
crontab -e
```

Add:

```bash
*/10 * * * * /Users/username/Downloads/result_cron/venv/bin/python /Users/username/Downloads/result_cron/neetpg_round3_checker.py >> /Users/username/Downloads/result_cron/neetpg.log 2>&1
```

---

## 🛑 Stop / Pause Cron Job

### Pause:

```bash
crontab -e
# comment the line using #
```

### Remove completely:

```bash
crontab -r
```

---

## 🛡️ Reliability Features

* Timeout protection (prevents hanging)
* Browser-like User-Agent (avoids blocking)
* Duplicate notification prevention
* Safe failure (cron retries on next run)

---

## ♻️ Reusing for Any Exam (Checklist)

To adapt this project:

1. Change `URL`
2. Update `KEYWORDS`
3. Rename state file (optional)
4. Update email subject text
5. Adjust cron frequency if needed

That’s it — no other code changes required.

---

## 🔧 Optional Enhancements

* Telegram / WhatsApp alerts
* macOS desktop notifications
* Auto-download result PDF
* launchd instead of cron (macOS-native)
* Multiple mirror URLs

---

## ⚖️ Disclaimer

* This tool only accesses **publicly available webpages**
* Uses low-frequency polling (ethical & safe)
* Intended for **personal notification**, not scraping

---

## ✅ Summary

This system ensures:

* You **never miss a result**
* No manual refreshing
* No dependency on server speed
* Fully reusable for future exams

Once set up, it runs silently in the background and alerts you the moment results go live.
