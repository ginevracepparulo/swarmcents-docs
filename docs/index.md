# SwarmCents Documentation

**SwarmCents** is an autonomous AI agent system developed to operate within **Torus’s agentic AI ecosystem**. It transforms unstructured social media content into structured, verifiable predictions, enabling deeper insight into public sentiment and prediction accuracy. The system leverages large language models (LLMs), API integrations, and multi-agent orchestration to extract, analyze, and validate prediction data.

---

## Overview of Agents

### 1. **Prediction Finder Agent**

**Purpose:**
Identify and extract explicit or implicit predictions from social media (X/Twitter) related to a user-defined Polymarket topic.

**Workflow:**

- Extracts Polymarket topic from the user's prompt using an LLM.
- Generates a Datura API search query.
- Retrieves relevant tweets via the Datura API.
- Processes and structures tweet data.
- Uses an LLM to detect explicit and implicit predictions.
- Filters and returns tweets containing qualified predictions.

**User Prompt Example:**

> "Find predictions on who will win US presidential elections in 2025?"

**Output Format:**

```json
{
  "username": "...",
  "favourites_count": ...,
  "is_blue_verified": ...,
  "tweet_text": "...",
  "like_count": ...,
  "created_at": "...",
  "tweet url": "..."
}
```

---

### 2. **Prediction Profiler Agent**

#### Functionality 1: **Predictor Profile Builder**

**Purpose:**
Generate a profile for an X user based on their prediction behavior and topics.

**Workflow:**

- Retrieves recent tweets for a given user.
- Batches and analyzes tweets using an LLM to detect predictions.
- Filters out non-predictive tweets.
- Identifies patterns in predictions: topics, confidence levels, styles, and behavioral trends.

**User Prompt Example:**

> "Build profile for @elonmusk."

**Output Format:**

```json
{
  "handle": "...",
  "total_tweets_analyzed": ...,
  "prediction_tweets": [...],
  "prediction_count": ...,
  "prediction_rate": ...,
  "analysis": {
    "topics": {
      "politics": ...,
      "crypto": ...,
      ...
    },
    "confidence_level": "...",
    "prediction_style": "...",
    "patterns": ["...", "..."],
    "summary": "..."
  }
}
```

#### Functionality 2: **Credibility Score Calculator**

**Purpose:**
Evaluate the reliability of an X user's predictions by verifying their historical accuracy.

**Workflow:**

- Generates predictor profile.
- Verifies past predictions using the Prediction Verifier Agent.
- Calculates a credibility score: `true / total predictions`.
- Outputs user statistics, verified predictions, and a credibility summary.

**User Prompt Example:**

> "Calculate the credibility score for @elonmusk."

**Output Format:**

```json
{
  "handle": "...",
  "credibility_score": ...,
  "prediction_stats": {
    "total": ...,
    "true": ...,
    "false": ...,
    "uncertain": ...
  },
  "verified_predictions": [...],
  "profile_summary": "..."
}
```

---

### 3. **Prediction Verifier Agent**

**Purpose:**
Assess the truth value of a given prediction related to a Polymarket topic using external information sources.

**Workflow:**

- Extracts relevant Polymarket topic using an LLM.
- Searches for confirmation using:

  - Google Search API
  - Datura Web Search API

- Analyzes source data to determine veracity via LLM.
- Labels prediction as **True**, **False**, or **Uncertain** based on evidence.

**User Prompt Example:**

> "Trump will win in 2025."

**Output Format:**

```json
{
  "result": "...",  // True, False, or Uncertain
  "summary": "...",
  "sources": [...]
}
```

---

## System Highlights

- **Agent Autonomy:** Each agent operates with minimal user input and performs multi-step reasoning and action-taking.
- **LLM-Powered Reasoning:** LLMs are used at multiple decision points—topic extraction, prediction identification, behavioral analysis, and result synthesis.
- **API Integration:** Seamless access to Datura and Google APIs ensures robust data gathering and verification.
- **Structured Insights:** Outputs are designed for downstream consumption in applications like dashboards, analytics engines, or credibility trackers.

---

## Usage Examples

1. **Discovering Predictions**

```bash
User: Find predictions on Ethereum price by Q4 2025
System: [Returns structured tweets containing related predictions]
```

2. **Building a Prediction Profile**

```bash
User: Build profile for @naval
System: [Returns analysis including topics, style, confidence, and summary]
```

3. **Calculating Credibility**

```bash
User: Calculate the credibility score for @balajis
System: [Returns credibility score and verification details]
```

4. **Verifying a Prediction**

```bash
User: Bitcoin will hit $100k by end of 2024
System: [Returns verification status, summary, and evidence sources]
```

---

## System Architecture Diagram

```plaintext
+--------------------------+
|    User Input Prompt     |
+------------+-------------+
             |
             v
+------------+-------------------+
|     Agent Dispatcher           |
+---+------------+--------+------+
|               |              |
v               v              v
+-----------+ +-----------+ +-----------+
|  Finder   | |  Profiler | | Verifier  |
|  Agent    | |  Agent    | |  Agent    |
+-----------+ +-----------+ +-----------+
                   |               ^
                   | (calls)       |
                   +---------------+

Outputs from all agents:
- Finder ➜ Structured Predictions
- Profiler ➜ Predictor Profiles & Credibility Scores
- Verifier ➜ Verified Prediction Outcomes

             v
+------------------------------------------+
|            Final Structured Output        |
|  (Predictions, Profiles, Verifications)   |
+------------------------------------------+
```

This diagram illustrates the multi-agent orchestration across components to convert social media predictions into actionable intelligence.
