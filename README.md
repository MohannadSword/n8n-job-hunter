# 🚀 n8n Automated Job Hunter

An automated workflow built with **n8n** that collects job listings from multiple sources, filters relevant IT & remote jobs, removes duplicates, and sends clean reports to Discord.

---

## 📌 Overview

This project automates the job search process by:

* Fetching jobs from RSS feeds (e.g., Remotive, WeWorkRemotely)
* Filtering only **IT-related** and **remote jobs**
* Removing duplicate listings using persistent memory
* Formatting results into readable messages
* Sending alerts directly to a Discord channel

---

## ⚙️ Features

* ⏰ Runs automatically using Cron (every hour)
* 🌐 Supports multiple job sources (RSS-based)
* 🧠 Smart filtering (categories + keywords ready)
* 🚫 Deduplication using GUID (persistent across runs)
* 📩 Sends formatted messages to Discord
* ✂️ Handles Discord message limits (auto-splitting)

---

## 🧩 Workflow Architecture

```text
Cron
  ↓
RSS Feeds (Remotive + WWR)
  ↓
Set (Add Source)
  ↓
Merge (Append)
  ↓
Set (Normalize Data)
  ↓
IF (Filter IT / Remote Jobs)
  ↓
Code (Remove Duplicates)
  ↓
Code (Format Messages)
  ↓
Discord (Send Alerts)
```

---

## 🛠️ Setup Instructions

### 1. Import Workflow

* Download the JSON workflow from `/workflows`
* Import it into your n8n instance

---

### 2. Configure RSS Sources

* Add RSS feed URLs (e.g. Remotive, WeWorkRemotely)
* Ensure nodes are connected properly

---

### 3. Configure Discord

1. Create a Discord bot or connect via Discord node credentials
2. Select your server and channel
3. In the **Message field**, use:

```javascript
{{$json.message}}
```

---

### 4. Activate Workflow

* Click **Publish**
* Ensure Cron is configured (e.g. every hour)

⚠️ Note:
Deduplication works **only when workflow is active**, not during manual runs.

---

## 🧠 Key Concepts

### Deduplication

* Uses `guid` (or fallback to `link`)
* Stored using n8n `staticData`
* Prevents repeated job alerts

---

### Data Normalization

All jobs are structured into a unified format:

```json
{
  "title": "...",
  "link": "...",
  "date": "...",
  "description": "...",
  "source": "...",
  "global UID": "...",
  "Categories": []
}
```

---

### Message Formatting

* Combines jobs into one report
* Splits messages to respect Discord’s 2000-character limit

---

## ⚠️ Common Issues

| Issue                   | Fix                                     |
| ----------------------- | --------------------------------------- |
| Duplicate jobs          | Ensure workflow is published            |
| No messages sent        | Check filtering conditions              |
| Discord error           | Message too long (handled in formatter) |
| Static data not working | Avoid manual execution                  |

---

## 🧹 Reset Deduplication

To clear stored job history:

```javascript
staticData.seenGuids = [];
```

---

## 📁 Project Structure

```text
n8n-job-hunter/
│
├── workflows/
│   └── job-hunter.json
│
├── README.md
│
├── .env.example
│
└── screenshots/
    └── workflow.png
```

---

## 🚀 Future Improvements

* 🔌 Add API-based job sources
* 🤖 AI-powered job summarization
* 📊 Store jobs in database (Notion / MongoDB)
* 🧠 Skill-based filtering
* 📅 Daily digest mode

---

## 🏁 Result

✔ Fully automated job discovery
✔ Clean, filtered job alerts
✔ No duplicate notifications
✔ Scalable and extensible system

---

## 📄 License

This project is open-source and free to use.
Under the MIT license
---

## 🤝 Contributing

Feel free to fork, improve, and adapt this workflow for your needs.

---

**Built with n8n ⚡**

