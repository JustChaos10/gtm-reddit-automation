# AI-Driven GTM Automation (Reddit → Google Sheets / Slack)

Automates discovery of relevant Reddit posts for GTM work, scores posts with AI, logs to Google Sheets, and alerts high‑priority items in Slack. This repo includes a Zapier export and a short how‑to so you can import, customize, and run the workflow quickly.

---

## What this workflow does

- **Searches Reddit** on a schedule for growth/retention topics you define
- **De‑dupes** via a saved “last processed” id (Storage by Zapier)
- **Filters** by freshness and keywords
- **AI analysis** → summary, intent, relevance (0–1), sentiment, and a suggested engagement
- **Branches**:
  - **Log to Google Sheets** for routine items
  - **Send Slack alert** for high‑priority posts (e.g., relevance ≥ 0.85 and intent is a question/complaint)
- **Updates** the saved id so you don’t process the same post twice

---

## Repo contents

- `exported-zap-2025-09-16T14_51_36.203Z.json` — Zapier export to recreate the workflow (triggers, filters, AI prompt, branching, Sheets mapping, Slack DM, storage get/set).
- `GTM_Automation_Workflow_Documentation.docx` — Human‑readable overview and setup notes.
- `README.md` — You’re here.

---

## Quick start (Zapier)

1. **Import the Zap**  
   In Zapier → **Create** → **Import from file** → select `exported-zap-2025-09-16T14_51_36.203Z.json`.

2. **Connect apps**  
   - Reddit (read access)  
   - Google Sheets  
   - Slack (for DMs or a channel)  
   - Built‑ins: **Storage**, **Formatter**, **AI by Zapier**

3. **Configure**  
   - **Reddit search query:** tune keywords to your ICP or niche  
   - **Google Sheet:** pick spreadsheet + worksheet with the headers below  
   - **Slack target:** DM recipient or alerts channel  
   - **Thresholds:** default branch gate is relevance **0.85** (edit in the Filter/Paths step)  
   - **Storage key:** keep a stable key (e.g., `last_id`) across the **Get** and **Set** steps

4. **Turn it on** and test with a couple of recent posts.

---

## Suggested Google Sheets schema

| Column | Field |
|---|---|
| A | Timestamp (localized) |
| B | Subreddit |
| C | Author |
| D | Post Link |
| E | Title |
| F | Body |
| G | AI Summary |
| H | Intent |
| I | Adjusted Relevance |
| J | Sentiment Weight |
| K | Final Score (optional duplicate for charting) |
| L | Suggested Engagement |

> Make sure your header names match what the Zap expects before turning it on.

---

## Architecture (at a glance)

1. **Trigger:** Reddit search query (edit keywords to fit your market)  
2. **Storage (Get last_id)** → **Filter (freshness & id)** → **Keyword filters**  
3. **AI step:** returns JSON `{ summary, intent, relevance, sentiment, suggested_engagement }`  
4. **Formatter:** normalize/weight fields; format timestamps (e.g., `Asia/Kolkata`)  
5. **Branching:**  
   - **Path A:** Log to **Google Sheets**  
   - **Path B:** **Slack** alert for priority items  
6. **Storage (Set last_id)** to advance the cursor

---

## Customizing

- **Keywords:** Change the Reddit query to track competitors, product areas, or pain points.
- **Intent classes:** Adjust the AI prompt if you want different labels (e.g., “lead”, “bug report”, “feature request”).
- **Thresholds & weights:** Modify relevance/intent/sentiment weighting and the branch cut‑off.
- **Outputs:** Add Webhooks to your CRM, route severe items to an on‑call channel, or log to a database.

---

## Troubleshooting

- **No Slack DM appears:** Your workspace may restrict DMs from apps. Switch to posting in a channel the app can access.  
- **Duplicates show up:** Ensure the **Storage Get/Set** steps use the same key and run in the right order.  
- **Sheet rows misaligned:** Confirm your **header names** match the mapping in the Sheets action.  
- **Hitting Reddit rate limits:** Reduce frequency or narrow your search query.

---

## Roadmap ideas

- Add Notion or Airtable as alternative sinks  
- Add a “reply template” library keyed by intent  
- Export daily/weekly CSV summary for analytics  
- Basic dashboard (e.g., Data Studio/Looker or a small Streamlit view)

---

## License

MIT (recommended). Create a `LICENSE` file with the MIT text if you plan to share this publicly.

---

## Credits

Built from a Zapier workflow template and internal docs for GTM listening and triage. Adapt freely to your stack.
