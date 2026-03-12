# 🌞 Solar Lead Finder — AI-Powered B2B Lead Generation

An automated pipeline that finds the personal email addresses of **CEOs and Owners** at Canadian solar companies — built with **n8n**, **OpenAI GPT-4**, **SerpAPI**, and **Google Sheets**.

> Built by [Yassine Haigane](https://github.com/haigane-ai) — AI Automation Developer based in Toronto, Canada.

---

## 🎯 What It Does

This workflow processes a list of **52 Canadian solar companies** from a Google Sheet and automatically:

1. Reads each company row from Google Sheets
2. Uses **SerpAPI** to search for the CEO/Owner's name and contact information
3. Sends the search results to **OpenAI GPT-4** for intelligent email extraction and formatting
4. Validates and routes results through conditional logic
5. Appends found emails back into Google Sheets — ready for outreach
6. Sends email alerts on errors to keep the pipeline healthy
7. Respects a **daily rate limit** to stay within API quotas

---

## 🏗️ Workflow Architecture

```
Manual Trigger
     │
     ▼
Edit Fields (config)
     │
     ▼
Get Rows from Google Sheets
     │
     ├──► Send Email ON ERROR (Gmail alert)
     │
     ▼
DAILY LIMIT CHECK
     │
     ▼
IF (row already processed?)
     │
     ▼
Loop Over Items (one company at a time)
     │
     ├──► Wait (rate limiting delay)
     │
     ▼
SerpAPI (search: "[Company] CEO email" etc.)
     │
     ▼
Message a Model — OpenAI GPT-4
(extract & format the personal email)
     │
     ▼
Edit Fields (normalize output)
     │
     ▼
Switch (route by result type)
     │
     ├──► Append Row in Sheet (new lead found ✅)
     ├──► Update Row in Sheet (update existing row)
     ├──► Send Email ON ERROR (alert on failure)
     └──► Stop and Error (hard stop if needed)
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation engine |
| [OpenAI GPT-4](https://openai.com) | AI email extraction & parsing |
| [SerpAPI](https://serpapi.com) | Google Search results via API |
| [Google Sheets](https://sheets.google.com) | Input company list + output lead storage |
| [Gmail](https://gmail.com) | Error alert notifications |
| JavaScript (n8n Code node) | Custom data transformation logic |

---

## ⚙️ Setup & Configuration

### Prerequisites

- [n8n](https://n8n.io) instance (cloud or self-hosted)
- OpenAI API key (GPT-4 access)
- SerpAPI key
- Google Sheets + Gmail OAuth credentials configured in n8n

### Steps

1. **Clone or import the workflow JSON** into your n8n instance
2. **Set up credentials** in n8n for:
   - OpenAI API
   - SerpAPI
   - Google Sheets OAuth2
   - Gmail OAuth2
3. **Prepare your Google Sheet** with the following columns:

| Column | Description |
|--------|-------------|
| `Company Name` | Name of the solar company |
| `Website` | Company website URL |
| `Status` | Processing status (auto-filled) |
| `CEO/Owner Name` | Target decision-maker (auto-filled) |
| `Email` | Found personal email (auto-filled) |
| `Email Confidence` | GPT-4 confidence score (auto-filled) |

4. **Configure the daily limit** in the `DAILY LIMIT` node to match your SerpAPI plan
5. **Update the Gmail node** with your sender/recipient email for error alerts
6. Click **Execute Workflow** ▶️

---

## 📊 Results

- **52 Canadian solar companies** processed
- Extracts **personal emails** of CEOs and Owners (not generic info@ addresses)
- Automatically logs each result to Google Sheets for CRM import
- Error-resilient: failed rows are flagged and emailed — workflow continues

---

## 💡 How the AI Email Extraction Works

The SerpAPI node queries Google Search with prompts like:
```
"[CEO Name] [Company] email"
"[Company] CEO contact"
```

The raw search results are passed to **GPT-4** with a prompt instructing it to:
- Identify the most likely personal email address
- Ignore generic contact@, info@, support@ addresses
- Return a confidence score
- Return `null` if no valid email is found

The `Switch` node then routes based on whether a valid email was found, updating or appending the sheet row accordingly.

---

## 🔒 Ethical & Legal Notes

- This tool is intended for **B2B outreach only** — targeting business decision-makers in a professional context
- All data collected is from **publicly available sources** via Google Search
- Respect CAN-SPAM and CASL regulations when sending outreach emails in Canada
- Do not use for spam or unsolicited mass emailing

---

## 🗺️ Roadmap

- [ ] Add LinkedIn scraping layer for higher email confidence
- [ ] Auto-send personalized cold email via Gmail after email is found
- [ ] Expand to other industries (HVAC, roofing, windows — home improvement)
- [ ] Add webhook trigger for real-time processing
- [ ] Package as a reusable n8n template for other agencies

---

## 👤 Author

**Yassine Haigane**  
AI Automation Developer | Toronto, Canada  
🔗 [GitHub: haigane-ai](https://github.com/haigane-ai)  
💼 Available for freelance AI automation, chatbot, and lead generation projects

---

## 📄 License

MIT License — free to use, modify, and distribute with attribution.
# 🔄 Solar Lead Finder — Workflow Diagram

## Full Node Flow
