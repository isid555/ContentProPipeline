# ContentPipeline Pro: Multi-Agent Automated Content Strategy & Triage System

A production-grade agentic workflow built in **n8n** that automates core content marketing operations. The system ingests a raw topic seed via Webhook, orchestrates a multi-agent content analysis pipeline, evaluates SEO competitiveness, estimates production complexity, and automatically routes opportunities either to a content backlog or for editorial escalation.

---

## 📌 Problem Statement

### Target Users

* Content Marketing Managers
* SEO Specialists
* Creative Directors
* Digital Marketing Agencies

### The Problem

Content strategy workflows often suffer from fragmented decision-making. Teams generate content ideas without considering SEO competitiveness, while SEO teams may overlook production effort and resource requirements. This creates bottlenecks, slows execution, and increases manual review overhead.

### Objective

Automate the complete content evaluation lifecycle:

**Topic Idea → Content Strategy → SEO Analysis → Production Assessment → Automated Routing**

### Expected Outcome

* Low-complexity, SEO-friendly content ideas are automatically added to a production backlog.
* High-effort or highly competitive content opportunities are automatically escalated for human review.
* Reduced manual screening effort.
* Faster content planning and execution cycles.

---

# 🏗️ Workflow Architecture

```text
[Webhook (Topic Input)]
            │
            ▼
[Content Planner Agent]
   (Generate 3 Subtopics)
            │
            ▼
[SEO Specialist Agent]
 (Keyword Difficulty Analysis)
            │
            ▼
[Creative Director Agent]
 (Production Complexity Review)
            │
            ▼
     [IF Validation]
            │
     ┌──────┴──────┐
     │             │
     ▼             ▼
[Google Sheet]   [Gmail Alert]
 (Approved)      (Escalated)
```

---

## ⚙️ Workflow Components

### 1. Webhook Trigger

#### Node

`Webhook (Topic Input)`

#### Purpose

Accepts incoming POST requests containing a content topic.

#### Example Input

```json
{
  "topic": "Generative AI applications in enterprise supply chain risk management"
}
```

---

### 2. Content Planner Agent

#### Role

Acts as the Content Strategist.

#### Responsibilities

* Analyze the incoming topic.
* Generate exactly three content pillars/sub-topics.
* Structure the output for downstream processing.

#### Example Output

```json
{
  "sub_topics": [
    "AI-powered supplier risk prediction",
    "Demand forecasting using machine learning",
    "Automated logistics optimization"
  ]
}
```

---

### 3. SEO Specialist Agent

#### Role

Acts as the SEO Analyst.

#### Responsibilities

* Evaluate keyword competitiveness.
* Identify competitor positioning.
* Estimate ranking difficulty.

#### Output Schema

```json
{
  "keyword_difficulty": "Medium",
  "competitor_angles": [
    "Industry reports",
    "Case studies",
    "Implementation guides"
  ]
}
```

#### Allowed Difficulty Values

* Low
* Medium
* High

---

### 4. Creative Director Agent

#### Role

Acts as the Production Reviewer.

#### Responsibilities

* Estimate production effort.
* Evaluate asset requirements.
* Recommend distribution formats.

#### Output Schema

```json
{
  "production_score": 6,
  "recommended_formats": [
    "Blog",
    "LinkedIn Carousel",
    "Video"
  ]
}
```

#### Production Score Range

```text
1 = Very Easy
10 = Extremely Complex
```

---

## 🔀 Validation & Routing Logic

### Decision Criteria

Content proceeds automatically only if:

```text
production_score < 8
AND
keyword_difficulty != "High"
```

### Approved Path

If the content passes validation:

* Topic details are appended to Google Sheets.
* Content enters the production backlog.

### Escalation Path

If the content fails validation:

* An alert email is sent.
* Editorial stakeholders receive the content analysis and scores.
* Human review is required before production.

---

## 📊 Data Stored in Google Sheets

Each approved content opportunity stores:

| Field               | Description               |
| ------------------- | ------------------------- |
| Topic               | Original user topic       |
| Sub Topics          | Generated content pillars |
| Keyword Difficulty  | SEO competitiveness score |
| Production Score    | Resource effort estimate  |
| Recommended Formats | Suggested content formats |
| Timestamp           | Submission time           |

---

## 📧 Escalation Email Contents

The workflow automatically includes:

* Original topic
* Generated sub-topics
* SEO difficulty assessment
* Production score
* Recommended formats
* Reason for escalation

Example:

```text
Action Required: Content Review

Topic:
Generative AI applications in enterprise supply chain risk management

Keyword Difficulty:
High

Production Score:
9

Reason:
High SEO competition and elevated production requirements.
```

---

# 🛠️ Tech Stack

| Component              | Technology              |
| ---------------------- | ----------------------- |
| Workflow Orchestration | n8n                     |
| AI Model               | Llama-3.3-70B-Versatile |
| AI Provider            | Groq                    |
| Data Storage           | Google Sheets           |
| Notifications          | Gmail                   |
| Trigger Layer          | Webhook API             |
| Routing Logic          | n8n IF Node             |

---

## 🤖 Agent Configuration

| Agent             | Responsibility                           |
| ----------------- | ---------------------------------------- |
| Content Planner   | Topic decomposition and content ideation |
| SEO Specialist    | Keyword competitiveness analysis         |
| Creative Director | Production complexity estimation         |

All agents return structured JSON outputs to ensure deterministic downstream execution.

---

# 🚀 Installation & Setup

## Prerequisites

* n8n (Cloud or Self-hosted)
* Groq API Key
* Google Sheets Access
* Gmail Access

---

## Step 1: Import Workflow

Import the provided JSON workflow into your n8n workspace.

---

## Step 2: Configure Credentials

### Groq

Add your:

* API Key

### Google Sheets

Authorize:

* Google Drive API
* Google Sheets API

### Gmail

Authorize:

* Gmail OAuth2

---

## Step 3: Activate Workflow

1. Enable the workflow.
2. Copy the webhook URL.
3. Send a POST request.

Example:

```bash
curl -X POST https://your-n8n-instance/webhook/content-pipeline-pro \
-H "Content-Type: application/json" \
-d '{
  "topic":"Generative AI applications in enterprise supply chain risk management"
}'
```

---

# 📈 Business Benefits

### Reduced Manual Review

Only high-risk content ideas require human intervention.

### Faster Content Planning

Content opportunities are automatically analyzed and categorized.

### Better SEO Alignment

Keyword competitiveness is evaluated before production begins.

### Improved Resource Allocation

Production-heavy campaigns are identified early.

---

# 🔒 Reliability Features

* Structured JSON Outputs
* Deterministic Routing Logic
* Human-in-the-Loop Escalation
* Automated Data Logging
* AI + Rule-Based Decision Making

---

# 🙋‍♂️ Individual Contribution

This project was designed, implemented, tested, and documented independently.

Contributions include:

* Workflow architecture design
* Prompt engineering
* Agent orchestration
* JSON schema design
* Conditional routing logic
* Google Sheets integration
* Gmail notification system
* End-to-end workflow testing and debugging

---

## License

This project is intended for educational and portfolio demonstration purposes.
