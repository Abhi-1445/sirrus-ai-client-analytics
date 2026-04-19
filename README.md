# Sirrus.ai — Client Analytics Review
### End-to-End Business Analytics | MySQL · Python · Power BI

> **Context:** Sirrus.ai is a PropTech SaaS CRM platform serving real estate developers. This project was built as part of the Data Analytics function at TCG (The Connect Group) to monitor client health, detect churn risk, and surface operational gaps across the platform.

---

## Project Overview

This repository contains a full-stack analytics project that analyzes Sirrus.ai's client base across seven key dimensions — from call quality and AI failures to feature adoption, onboarding health, and support ticket resolution. The work spans raw SQL queries, Python-based EDA, and a Power BI dashboard designed for executive review.

---

## Repository Structure

```
sirrus-ai-client-analytics/
│
├── SQL analysis                    # MySQL queries (7 analytical modules)
├── python analysis/                # Jupyter notebooks — EDA & visualization
├── python analysis output charts/  # Exported chart PNGs from Python
├── Dashboard-images/               # Power BI dashboard screenshots
└── Sirrus.ai Client Analytics Review.pbix   # Full Power BI report
```

---

## Analytics Modules

### 1. Call Health & AI Summary Failures
Identifies clients with the highest AI call summary failure rates and dropped call volumes over a rolling 7-day window. Used to flag Telephone partner reliability issues and prioritize engineering escalations.

**Key metrics:** `total_calls`, `dropped_calls`, `ai_failed`, `failure_rate_pct`

---

### 2. Churn Risk Detection
Flags clients who have gone silent — no logins in the past 7–14 days — and assigns a risk tier (HIGH / MEDIUM / ACTIVE). Helps account managers proactively reach out before churn occurs.

**Risk tiers:**
- `HIGH RISK` → silent > 14 days
- `MEDIUM RISK` → silent > 7 days
- `ACTIVE` → logged in within 7 days

---

### 3. AI Pair Score Distribution
Buckets all calls by `pair_score` (0–100) to understand the quality distribution of AI-assisted call summaries. Cross-referenced with call duration to identify whether longer calls produce better scores.

**Score buckets:** `0–25 Low` · `26–50 Medium` · `51–75 High` · `76–100 Very High`

---

### 4. AI Summary Failure Impact (Client-Level)
Measures the scale of the AI summary failure problem at the individual client level — total calls, failed summaries, and failure percentage — to understand which clients are most affected and quantify revenue impact risk.

---

### 5. Call Duration vs. Pair Score Correlation
Checks whether longer calls correlate with higher AI pair scores. Results inform whether call duration is a reliable proxy for call quality in the Sirrus.ai platform.

**Duration buckets:** `Under 2 min` · `2–5 min` · `5–10 min` · `10+ min`

---

### 6. Feature Adoption by Module
Aggregates feature usage by client across three modules — `PreSales`, `Marketing`, `PostSales` — over the last 30 days. Identifies which clients are underusing the platform and which modules have the lowest adoption, informing CSM outreach and product decisions.

**Output used for:** Python heatmap visualization

---

### 7. Onboarding Health (First 30 Days)
Measures new client activity in their first 30 days post-onboarding: calls made and features explored. Used to identify clients who had a weak onboarding and are at risk of early churn.

---

### 8. Support Ticket Analysis
Breaks down support tickets by category — volume, average resolution time (in hours), and count of still-open tickets. Reveals the biggest complaint categories and resolution bottlenecks.

---

## Tech Stack

| Tool | Purpose |
|---|---|
| **MySQL** | Raw data queries across `clients`, `calls`, `user_logins`, `feature_logs`, `tickets` tables |
| **Python (Pandas, Matplotlib, Seaborn, Plotly)** | EDA, metric calculation, chart generation |
| **Power BI** | KPI dashboard for executive review |
| **Excel** | Data validation and supplementary reporting |

---

## Database Schema (sirrus_tcg)

```
clients         — client_id, company_name, city, account_manager, onboarding_date
calls           — call_id, client_id, call_date, call_status, call_duration_sec,
                  ai_summary_status, pair_score
user_logins     — login_id, client_id, login_time
feature_logs    — log_id, client_id, user_id, feature_name, module, used_at
tickets         — ticket_id, client_id, category, status, created_at, resolved_at
```

---

## Key Insights

- Clients with **AI summary failure rates above 40%** were concentrated in specific cities, pointing to regional Telephone partner issues rather than platform-wide failures.
- **14% of the client base** had not logged in for over 14 days, qualifying as HIGH churn risk at the time of analysis.
- **Pair scores** showed a weak positive correlation with call duration — calls under 2 minutes averaged significantly lower scores, suggesting minimum call thresholds for reliable AI summaries.
- **PostSales module** had the lowest adoption across clients, with most usage concentrated in PreSales — an indicator of underutilized upsell and retention tooling.
- Clients onboarded with **fewer than 3 calls in their first 30 days** showed higher churn rates in subsequent months.
- **Billing and Integration** were the top two support ticket categories by volume, with average resolution times exceeding 18 hours.

---

## Power BI Dashboard

The `.pbix` file contains a 4-page executive dashboard. Each page is filterable by **Date Range** and **City**.

---

### 📊 Platform Health
> Total Calls · AI Success Rate · Telephone Partner Failure Rate · Call Outcome Breakdown · AI Summary Status by City

![Platform Health](https://raw.githubusercontent.com/Abhi-1445/sirrus-ai-client-analytics/main/Dashboard-images/Platform%20health.png)

---

### 👥 Client Activity
> Active Clients · Churn Risk Clients · YRR Total · Client-level status table with Subscription Plan, Onboarding Date & Inactive Days

![AI Performance](https://raw.githubusercontent.com/Abhi-1445/sirrus-ai-client-analytics/main/Dashboard-images/AI%20Performance.png)

---

### 🤖 AI Performance
> AI Success Rate · Best & Lowest City · Avg Pair Score by City · Call Value trend by Month

![Client Activity](https://raw.githubusercontent.com/Abhi-1445/sirrus-ai-client-analytics/main/Dashboard-images/Client%20Activity.png)

---

### 🎫 Support Tickets
> Open Tickets · Total Tickets Raised · Tickets by Category · Avg Resolution Time (Hours) by Category

![Support Tickets](https://raw.githubusercontent.com/Abhi-1445/sirrus-ai-client-analytics/main/Dashboard-images/Support%20Tickets.png)
---

## How to Use

1. Clone the repository
2. Run the SQL queries in `SQL analysis` against your `sirrus_tcg` MySQL database
3. Open the notebooks in `/python analysis` — update data paths as needed
4. Open `Sirrus.ai Client Analytics Review.pbix` in Power BI Desktop to explore the dashboard

---

## Author

**Abhishek** — Data Analytics, TCG (The Connect Group)
Working on Sirrus.ai · PropTech SaaS · Real Estate CRM Analytics

---

*Built for internal client health monitoring and CEO-level reporting.*
