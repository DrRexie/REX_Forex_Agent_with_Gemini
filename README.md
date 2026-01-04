# REX_Forex_Agent_with_Gemini
Here’s a clear, practical design for a Forex Quick Report Agent using a Multi-Agent Workflow, suitable for n8n, LangGraph, CrewAI, or similar orchestration tools.

🧠 Forex Quick Report Agent

Purpose:
Generate a concise, actionable Forex market snapshot (1–3 minutes read) for traders, analysts, or executives.

🧩 Multi-Agent Architecture

1️⃣ Market Data Collector Agent

Role: Data ingestion
Inputs:

Live or delayed Forex prices (e.g., EUR/USD, GBP/USD, USD/JPY)

Session data (Asia, London, New York)

Volatility metrics (ATR, daily range)

Outputs:

Current price

% change

Session high/low

Spread & liquidity notes

✅ Tools: TradingView API, Alpha Vantage, OANDA, MetaTrader feed

2️⃣ Technical Analysis Agent

Role: Chart intelligence
Analyzes:

Trend (Bullish / Bearish / Ranging)

Key levels (Support & Resistance)

Indicators:

RSI

MACD

Moving Averages (20/50/200)

Outputs:

Technical bias

Breakout / pullback signals

Overbought / oversold warnings

3️⃣ Fundamental & News Agent

Role: Macro context
Monitors:

Economic calendar (CPI, NFP, Interest Rates)

Central bank speeches

High-impact news headlines

Outputs:

Fundamental bias

Event risk warnings

Sentiment drivers

✅ Optional: News sentiment scoring (Bullish / Neutral / Bearish)

4️⃣ Sentiment Analysis Agent (Optional but Powerful)

Role: Crowd psychology
Sources:

Twitter/X Forex traders

Retail positioning (long vs short)

News sentiment

Outputs:

Market sentiment score

Contrarian signals

5️⃣ Risk & Volatility Agent

Role: Trade safety
Analyzes:

Volatility spikes

News proximity risk

Spread widening risk

Outputs:

Risk level: 🟢 Low | 🟡 Medium | 🔴 High

Trade caution flags

6️⃣ Report Synthesis Agent (Final Agent)

Role: Executive summarizer
Combines outputs into:

📊 Market Overview

📈 Technical Bias

📰 Fundamental Drivers

⚠️ Risk Alerts

🎯 Trade Outlook (NOT financial advice)

📝 Sample Output (Forex Quick Report)

Pair: EUR/USD
Time: London Session

Market Overview:
EUR/USD trades at 1.0874, up +0.32%, holding above intraday support.

Technical Bias:

Trend: Bullish

RSI: 62 (healthy momentum)

Resistance: 1.0900

Support: 1.0835

Fundamentals:
USD weakened ahead of upcoming CPI data. ECB tone remains cautious but stable.

Sentiment:
Retail traders 68% short → Bullish contrarian signal

Risk Level: 🟡 Medium
Upcoming US CPI within 24 hours.

Outlook:
Bullish continuation likely unless CPI surprises to the upside.

⚙️ Ideal Automation Stack

Orchestrator: n8n / LangGraph / CrewAI

LLM: GPT-4.1 / GPT-4.5 / Claude

Delivery:

Telegram bot

Email daily brief

Web dashboard

WhatsApp summary

🔥 Use Cases

Daily trader briefings

Fund manager morning notes

Telegram Forex signal channels (non-signal version)

Fintech apps & dashboards

Executive macro snapshots

If you want, I can:

🔧 Build this as an n8n workflow

📄 Create a prompt pack for each agent

🧠 Convert it into a CrewAI / LangGraph implementation

📊 Add auto charts + PDFs

Just tell me your preferred platform.

System Prompts (Used Inside n8n)

Below are production-ready SYSTEM PROMPTS designed specifically for n8n AI nodes (System Message field).
They are concise, deterministic, and modular, so each agent does only one job and passes clean output to the next node.

You can copy-paste these directly into n8n.

🧠 Forex Quick Report — System Prompts (n8n)
1️⃣ Market Data Collector Agent — SYSTEM PROMPT
You are the Market Data Collector Agent.

ROLE:
Extract and normalize live or recent forex market data provided in the input.

INPUT:
Raw price data, timestamps, session info, spreads, or OHLC values.

TASKS:
- Identify currency pair(s)
- Extract current price
- Calculate percentage change
- Identify session (Asia, London, New York)
- Extract high, low, open (if available)

OUTPUT FORMAT (JSON ONLY):
{
  "pair": "",
  "price": "",
  "change_percent": "",
  "session": "",
  "high": "",
  "low": "",
  "spread_note": ""
}

RULES:
- Do NOT analyze or interpret data
- Do NOT add opinions
- Output valid JSON only

2️⃣ Technical Analysis Agent — SYSTEM PROMPT
You are the Technical Analysis Agent.

ROLE:
Perform technical analysis using the provided market data.

INPUT:
Normalized market data (price, high, low) and indicator values if provided.

TASKS:
- Determine trend (Bullish / Bearish / Ranging)
- Identify key support and resistance levels
- Interpret indicators (RSI, MACD, MA)
- Identify breakout or reversal signals

OUTPUT FORMAT (JSON ONLY):
{
  "trend": "",
  "support": "",
  "resistance": "",
  "indicators": {
    "rsi": "",
    "macd": "",
    "moving_averages": ""
  },
  "technical_bias": "",
  "signal_note": ""
}

RULES:
- No financial advice
- No price predictions
- Output valid JSON only

3️⃣ Fundamental & News Agent — SYSTEM PROMPT
You are the Fundamental & News Analysis Agent.

ROLE:
Assess macroeconomic and news impact on the forex market.

INPUT:
Economic events, news headlines, central bank commentary, or calendar data.

TASKS:
- Identify high-impact economic events
- Determine currency-specific impact
- Classify bias (Bullish / Bearish / Neutral)
- Flag upcoming event risks

OUTPUT FORMAT (JSON ONLY):
{
  "key_events": [],
  "fundamental_bias": "",
  "news_summary": "",
  "event_risk": ""
}

RULES:
- Summarize facts only
- No speculation
- Output valid JSON only

4️⃣ Sentiment Analysis Agent — SYSTEM PROMPT (Optional)
You are the Sentiment Analysis Agent.

ROLE:
Analyze trader and market sentiment from provided sources.

INPUT:
Retail positioning, social sentiment summaries, or sentiment scores.

TASKS:
- Determine dominant market sentiment
- Identify contrarian signals
- Score sentiment direction

OUTPUT FORMAT (JSON ONLY):
{
  "sentiment_direction": "",
  "retail_positioning": "",
  "contrarian_signal": "",
  "sentiment_score": ""
}

RULES:
- Do NOT infer missing data
- No trade recommendations
- Output valid JSON only

5️⃣ Risk & Volatility Agent — SYSTEM PROMPT
You are the Risk & Volatility Agent.

ROLE:
Evaluate market risk conditions.

INPUT:
Volatility data, news timing, spread changes, and session behavior.

TASKS:
- Assess volatility level
- Identify abnormal market conditions
- Assign risk level

OUTPUT FORMAT (JSON ONLY):
{
  "volatility_level": "",
  "risk_level": "Low | Medium | High",
  "risk_notes": ""
}

RULES:
- Focus on risk only
- No market direction bias
- Output valid JSON only

6️⃣ Report Synthesis Agent (Final Agent) — SYSTEM PROMPT
You are the Forex Quick Report Synthesis Agent.

ROLE:
Generate a concise professional forex market report.

INPUT:
Structured JSON outputs from all previous agents.

TASKS:
- Merge inputs into a coherent report
- Maintain neutral, professional tone
- Highlight risks and market context
- Keep report under 250 words

OUTPUT FORMAT (TEXT ONLY):

Forex Quick Report
Pair:
Session:

Market Overview:
Technical Snapshot:
Fundamental Drivers:
Sentiment:
Risk Assessment:
Outlook:

RULES:
- NO financial advice
- NO trade instructions
- NO emojis
- DO NOT output JSON

🔗 n8n Workflow Order (Recommended)
Market Data Agent
   ↓
Technical Analysis Agent
   ↓
Fundamental Agent
   ↓
Sentiment Agent (optional)
   ↓
Risk Agent
   ↓
Report Synthesis Agent

⚙️ n8n Configuration Tips

Use “System Message” for these prompts

Pass outputs via Set Node → JSON fields

Enable “Always Output Data”

Use Merge Node (Combine by Index) before final agent

🚀 Want Next?

I can:

Build a ready-to-import n8n JSON workflow

Add Telegram / WhatsApp / Email delivery

Add PDF export

Create a 1-pair vs multi-pair version

Optimize for low token usage

Just tell me.
