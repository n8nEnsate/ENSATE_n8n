
# 🐦 Twitter/X Scraper — n8n Workflow

This is a fully automated **Twitter/X Scraper** built using **n8n**. It fetches tweets based on a search query, formats and processes the data, paginates results, and stores structured tweet metadata to a **Google Sheet**.

---

## 📌 Features

- 🔎 Twitter/X advanced search via API
- 📄 Extracts tweet metadata (likes, views, retweets, etc.)
- 🔁 Pagination with cursor control
- 🗃️ Saves to Google Sheet (with OAuth2)
- 🔂 Limited to 3 iterations/pages (modifiable)

---

## 📁 Workflow Sections

### 🟩 Scraping X

| Node | Name | Purpose |
|------|------|---------|
| 🟢 **Manual Trigger** | `When clicking 'Test workflow'` | Starts the flow manually |
| 🟣 **Set Count** | Initializes `count = 1` |
| 🟣 **Counter** | Stores current `count` and `cursor` |
| 🌐 **Get Tweets** | Sends HTTP request to Twitter API with cursor & query |
| 🧠 **Extract Info** | JS code node that extracts & cleans tweet data |
| 📄 **Append Row in Sheet** | Adds tweet data into Google Sheet |

### 🟨 Checking Count

| Node | Name | Purpose |
|------|------|---------|
| 🔢 **If** | Checks if `counter == 3` (max 3 pages) |
| ⏹️ **NoOp** | Stops flow if max is reached |

### 🟦 Increasing Count & Cursor

| Node | Name | Purpose |
|------|------|---------|
| 🔁 **Limit** | Limits to 1 item for pagination |
| ✍️ **Set Increase** | Saves `cursor` from last Twitter page |
| ➕ **Increase Count** | Increments `counter` (JS node) |
| ✍️ **Set Count and Cursor** | Updates the `counter` and `cursor` for next loop |

---

## 🔧 Tweet Fields Collected

Each tweet pushed to the Google Sheet contains the following:

| Field | Description |
|-------|-------------|
| `Tweet ID` | Unique ID of the tweet |
| `URL` | Direct link to the tweet |
| `Content` | Tweet text content |
| `Likes` | Like count |
| `Retweets` | Retweet count |
| `Replies` | Reply count |
| `Quotes` | Quote count |
| `Views` | View count |
| `Date` | Creation date (formatted) |

---

## 📊 Google Sheet Integration

Tweets are exported to a Google Sheet connected via OAuth2. The columns match the extracted tweet fields. Example output:

| Tweet ID | URL | Content | Likes | Retweets | Replies | Quotes | Views | Date |
|----------|-----|---------|-------|----------|---------|--------|-------|------|
| 123456 | https://x.com/... | "OpenAI launches GPT..." | 1500 | 450 | 90 | 30 | 25K | July 19, 2025 |

---

## 🔁 Pagination Logic

- The flow repeats itself **up to 3 times** using a `count` variable.
- Each loop retrieves a new `cursor` from Twitter API to fetch the next page.
- You can increase or decrease the `count` limit by editing the `If` node.

---

## 🛠️ Setup Instructions

1. **Import the JSON** into your n8n instance.
2. Configure:
   - 🔐 **Twitter API credentials** (Header Auth)
   - 📊 **Google Sheets OAuth2 credentials**
3. Modify search query in **Get Tweets** node if needed.
4. Click **"Execute workflow"** to start scraping.

---

## 💡 Customization Tips

- Change `"OpenAI"` in `Get Tweets` node to target different keywords.
- Modify `count == 3` in `If` node for deeper pagination.
- Add more fields if the Twitter API returns them.

---

## 🧑‍💻 Author

**Anas Elhassouni**  
*big data & AI Engineer in Progress*  
GitHub: [@ANASKITE8](https://github.com/)  
n8n / JS / Automation

---

## 📄 License

This project is open-source under the **MIT License**.
TEST