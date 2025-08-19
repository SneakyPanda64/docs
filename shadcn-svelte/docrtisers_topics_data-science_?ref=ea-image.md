# EthicalAds API Documentation

> **Disclaimer** – This documentation is a *sanitized* and *reformatted* version of the original marketing copy.  
> All examples that appeared in the source text are preserved verbatim.

---

## 1. Overview

EthicalAds is a privacy‑first ad network that serves ads only to developers and technical audiences, with a focus on AI/ML professionals.  
The API lets you:

* Create and manage ad campaigns
* Retrieve publisher and targeting data
* Calculate expected spend
* Access pricing and prospectus information

All traffic is cookie‑free and the network is part of the Acceptable Ads program, so you never pay for blocked impressions.

---

## 2. Authentication

All endpoints require an API key passed in the `Authorization` header:

```
Authorization: Bearer <YOUR_API_KEY>
```

> **Tip** – Keep your API key secret. It is tied to your account and billing.

---

## 3. Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/v1/publishers` | List publishers in the network |
| `GET` | `/v1/publishers/{id}` | Get details for a single publisher |
| `GET` | `/v1/targeting/geography` | List available geographic zones |
| `GET` | `/v1/targeting/topics` | List available topic categories |
| `POST` | `/v1/campaigns` | Create a new campaign |
| `GET` | `/v1/campaigns/{id}` | Retrieve a campaign |
| `GET` | `/v1/pricing` | Retrieve current pricing tiers |
| `GET` | `/v1/campaigns/{id}/report` | Get a daily report for a campaign |

> All responses are JSON.

---

## 4. Data Models

### 4.1 Publisher

```json
{
  "id": "textacy",
  "name": "TextaCy",
  "category": "Docs",
  "url": "https://textacy.com"
}
```

> **Example** – The network includes publishers such as **TextaCy**, **Docs**, **Jupyter Project**, **Project**, **Numba**, **Docs**.

### 4.2 Geographic Zone

```json
{
  "code": "US",
  "name": "United States"
}
```

> **Example** – Geographic focus options:  
> * Run of Network – US, Canada, UK, Australia, New Zealand, Ireland  
> * Western Europe  
> * Asia Pacific  
> * Global

### 4.3 Topic

```json
{
  "id": "ai_ml",
  "name": "AI and Machine Learning"
}
```

> **Example** – Topic focus options:  
> * AI and machine learning  
> * Backend web development  
> * Frontend web development  
> * Security and privacy  
> * DevOps  
> * Custom niche targeting

### 4.4 Campaign

```json
{
  "id": "c12345",
  "name": "AI‑Focused Campaign",
  "status": "active",
  "budget": 5000,
  "currency": "USD",
  "geo_targets": ["US", "CA"],
  "topic_targets": ["ai_ml"],
  "creative": {
    "type": "image",
    "url": "https://example.com/ad.png",
    "alt_text": "AI ad"
  },
  "start_date": "2025-09-01",
  "end_date": "2025-09-30"
}
```

---

## 5. Pricing

Pricing is tiered by monthly spend and geography. The API returns the current rates:

```json
{
  "tiers": [
    {
      "min": 0,
      "max": 1000,
      "rate": 0.50
    },
    {
      "min": 1000,
      "max": 3000,
      "rate": 0.45
    },
    {
      "min": 3000,
      "max": 5000,
      "rate": 0.40
    },
    {
      "min": 5000,
      "max": 25000,
      "rate": 0.35
    },
    {
      "min": 25000,
      "max": null,
      "rate": 0.30
    }
  ],
  "discounts": {
    "3k_plus": 0.10,
    "25k_plus": 0.15
  }
}
```

> **Example** – A campaign with a $5,000 monthly budget receives a 10 % discount; a $30,000 budget receives a 15 % discount.

---

## 6. Campaign Creation

### 6.1 Request

```http
POST /v1/campaigns
Content-Type: application/json
Authorization: Bearer <YOUR_API_KEY>

{
  "name": "AI‑Focused Campaign",
  "budget": 5000,
  "currency": "USD",
  "geo_targets": ["US", "CA"],
  "topic_targets": ["ai_ml"],
  "creative": {
    "type": "image",
    "url": "https://example.com/ad.png",
    "alt_text": "AI ad"
  },
  "start_date": "2025-09-01",
  "end_date": "2025-09-30"
}
```

### 6.2 Response

```json
{
  "id": "c12345",
  "status": "pending",
  "created_at": "2025-08-20T12:34:56Z"
}
```

> **Note** – The campaign is initially `pending` until an ad specialist reviews it.  
> If you prefer a self‑serve flow, set `"self_serve": true` in the payload.

---

## 7. Campaign Reporting

```http
GET /v1/campaigns/c12345/report
Authorization: Bearer <YOUR_API_KEY>
```

```json
{
  "campaign_id": "c12345",
  "date": "2025-09-01",
  "impressions": 120000,
  "clicks": 2400,
  "spend": 480.00,
  "geo_breakdown": {
    "US": 70000,
    "CA": 50000
  },
  "topic_breakdown": {
    "ai_ml": 120000
  }
}
```

---

## 8. FAQ (Extracted from the original FAQ)

| Question | Answer |
|----------|--------|
| **Will my ads be blocked?** | We are part of the Acceptable Ads program, so ads are shown through many ad blockers. You only pay when your ad is shown. |
| **How much does it cost?** | Pricing varies by topic and geography. See `/v1/pricing`. |
| **What are my advertising goals?** | You can specify goals such as brand awareness, lead generation, or app installs. |
| **How did you hear about us?** | Options include Search Engine / Google, AI Chatbot / ChatGPT, Hacker News, Reddit, Slack group, Saw EthicalAds on another website, Other. |

---

## 9. Example Workflow (Python)

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://api.ethicalads.io/v1"

headers = {"Authorization": f"Bearer {API_KEY}"}

# 1. List publishers
resp = requests.get(f"{BASE_URL}/publishers", headers=headers)
publishers = resp.json()
print("Publishers:", publishers)

# 2. Create a campaign
campaign_payload = {
    "name": "AI‑Focused Campaign",
    "budget": 5000,
    "currency": "USD",
    "geo_targets": ["US", "CA"],
    "topic_targets": ["ai_ml"],
    "creative": {
        "type": "image",
        "url": "https://example.com/ad.png",
        "alt_text": "AI ad"
    },
    "start_date": "2025-09-01",
    "end_date": "2025-09-30"
}
resp = requests.post(f"{BASE_URL}/campaigns", json=campaign_payload, headers=headers)
campaign = resp.json()
print("Created campaign:", campaign)

# 3. Get campaign report
campaign_id = campaign["id"]
report = requests.get(f"{BASE_URL}/campaigns/{campaign_id}/report", headers=headers).json()
print("Report:", report)
```

---

## 10. Contact & Support

* **Email** – support@ethicalads.io  
* **Slack** – #ethicalads-support  
* **GitHub** – https://github.com/ethicalads

> All data is handled in compliance with the Privacy Policy and Terms of Service.

---