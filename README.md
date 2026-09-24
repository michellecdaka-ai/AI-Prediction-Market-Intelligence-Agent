# AI Prediction Market Intelligence Agent

An AI-powered workflow that transforms prediction-market questions into structured, evidence-based market analysis.

Built with **n8n, Polymarket market data, and an AI Agent**, the project explores how prediction-market data can be retrieved, evaluated, and converted into a concise analytical output.

---

## Overview

Prediction markets contain real-time information about how participants assess the likelihood of future events.

However, raw market data does not automatically provide a useful answer to a user's question. A system needs to determine which market is relevant, understand the market's status and resolution criteria, interpret outcome prices, and distinguish market signals from certainty.

This project was built to explore that process.

The agent accepts a natural-language prediction-market question, retrieves relevant Polymarket data, evaluates the available market information, and produces a structured analysis.

### Core workflow

**User Question → Market Data → Market Evaluation → AI Reasoning → Structured Analysis**

---

## What I Built

The workflow is designed to:

1. Receive a prediction-market question.
2. Extract and preserve the user's question.
3. Search Polymarket for relevant market information.
4. Evaluate potential markets using defined selection criteria.
5. Interpret Yes/No outcome prices as market-implied probabilities.
6. Consider supporting market information such as:
   - Volume
   - Liquidity
   - End date
   - Resolution criteria
   - Market status
   - Outcome prices
7. Separate market-implied probability from the agent's estimated probability.
8. Return the analysis in a structured format.

---

## System Architecture

```text
User Question
      ↓
Chat Trigger
      ↓
Edit Fields
      ↓
Polymarket API
      ↓
AI Agent
      ↓
Structured Output
      ↓
Market Analysis

Main components                    Component	Role
n8n	                            Workflow orchestration
Chat Trigger	                  Receives the user's question
Edit Fields	                    Standardizes the question for downstream processing
Polymarket API	                Provides prediction-market data
AI Agent	                      Evaluates market information and applies decision logic
Structured Output Parser	      Formats the agent's response
Edit Fields	                    Organizes the final output

Market Selection Logic

A key design principle was to avoid treating the first keyword match as the correct market.

The agent's market-selection logic considers:

- Relevance — Does the market directly correspond to the user's question?
- Market status — Is the market active/open?
- Resolution criteria — What exactly determines the outcome?
- End date — Does the market's timeframe correspond to the question?
- Outcome prices — What are the Yes/No prices?
- Volume and liquidity — How much activity and available trading depth does the market have?
- Evidence and uncertainty — What information supports the interpretation, and what remains uncertain?

The system is also designed to distinguish between a directly corresponding market and a market that is merely related to the question.

Understanding Market-Implied Probability

For a binary prediction market, the price of a Yes outcome can be interpreted as an approximate market-implied probability.

For example:
Yes price = $0.0285

Market-implied probability ≈ 2.85%

This should be interpreted as a market signal, not as a guaranteed or objectively correct probability.

The project therefore distinguishes between:
Market-implied probability
              ≠
Independent estimated probability

In the current test implementation, the successful Bitcoin example produced the same value for both fields. This is an important development finding and is documented as a limitation rather than being presented as an independent forecasting capability.

Real Test
Question

Will Bitcoin reach $150,000 at the end of 2026?

Relevant market identified

Will Bitcoin reach $150,000 by December 31, 2026?
| Metric                     |                Result |
| -------------------------- | --------------------: |
| Yes price                  |           **$0.0285** |
| Market-implied probability |           **≈ 2.85%** |
| Trading volume             |           **>$1.18M** |
| Liquidity                  |           **≈ $100K** |
| Market deadline            | **December 31, 2026** |

The workflow successfully identified a directly corresponding Polymarket market and returned structured evidence based on the retrieved market information.

Example Structured Output

The intended output structure is:
Question:
Will Bitcoin reach $150,000 at the end of 2026?

Relevant Market:
Will Bitcoin reach $150,000 by December 31, 2026?

Market-Implied Probability:
0.0285

Estimated Probability:
0.0285

Supporting Evidence:
- Directly corresponding Polymarket market
- Yes price of approximately $0.0285
- Volume above $1.18M
- Liquidity of approximately $100K
- Resolution date of December 31, 2026

Uncertainty / Risk:
Market prices represent participant expectations and can change over time.

Conclusion:
The current market signal implies approximately a 2.85% probability.

Research Framework

The project was built around several prediction-market concepts.

1. Market-implied probability

Outcome prices can provide a market-based estimate of the probability of an event occurring.

2. Volume

Volume represents the amount traded in the market and provides information about trading activity.

3. Liquidity

Liquidity relates to the amount of capital available for trading and can affect how easily positions can be entered or exited.

4. Resolution criteria

A market's wording and resolution rules determine what event actually counts as a Yes or No outcome.

5. End date

The timeframe of the market must correspond with the timeframe in the user's question.

6. Active vs. closed markets

The workflow is designed to prioritize active/current markets and avoid using closed or historical markets unless the user explicitly asks for them.

Testing & Validation

The workflow was tested using prediction-market questions, including a Bitcoin price question.

The successful test demonstrated that the system could:

Receive a natural-language question.
Search for relevant Polymarket information.
Identify a directly corresponding market.
Retrieve market information.
Interpret the Yes price as a market-implied probability.
Incorporate volume, liquidity, and end-date information.
Produce structured analytical output.

The test also exposed an important limitation:

The current implementation does not yet demonstrate a genuinely independent probability estimate because the estimated probability matched the market-implied probability in the successful test.

This finding informs the next stage of development.

Current Limitations

This is a working portfolio prototype rather than a production forecasting system.

Current limitations include:
The estimated probability is not yet independently generated from a separate forecasting methodology.
Market prices can change after data retrieval.
Search results may contain markets that are related but do not exactly match the user's question.
Prediction-market prices represent market signals rather than guaranteed outcomes.
The current workflow depends on the availability and structure of Polymarket data.
Bid-ask spread analysis is not yet incorporated into the probability calculation.
The system has not been evaluated across a large benchmark of prediction-market questions.

These limitations are intentionally documented as part of the project's development process.

Future Improvements

Potential next iterations include:

Independent probability estimation

Develop a separate evidence-based estimation layer rather than simply reproducing the market-implied probability.

Stronger market matching

Improve semantic matching between user questions and market wording.

Confidence and uncertainty analysis

Introduce a more explicit framework for uncertainty, evidence quality, and confidence.

Historical evaluation

Test the system against historical markets to evaluate how its estimates compare with eventual outcomes.

Market-quality signals

Incorporate additional market information such as bid-ask spread and other indicators of market conditions.

Broader market coverage

Extend the workflow to analyze multiple prediction-market questions and categories systematically.

Technology Stack
n8n — workflow automation and orchestration
Polymarket API — prediction-market data
AI Agent — natural-language reasoning and market analysis
Structured Output Parser — consistent response formatting
Google Gemini — AI model used in the workflow configuration
What This Project Demonstrates

This project demonstrates practical experience with:

AI workflow design
No-code / low-code automation
API integration
Data retrieval
Structured AI outputs
Prediction-market research
Probability interpretation
Market-data analysis
Prompt and agent design
Validation and testing
Documenting technical limitations
Iterative product development

More broadly, it demonstrates an approach of taking an open-ended question, connecting it to real external data, applying explicit decision logic, and turning the result into a structured analytical output.

Project Status

Status: Portfolio prototype completed and documented.

The core workflow has been built and tested, and the project has been documented through:

The exported n8n workflow
Detailed technical documentation
A visual project overview
This GitHub README

Future development will focus on improving independent probability estimation, validation, and market-selection robustness.

AI-Prediction-Market-Intelligence-Agent/
│
├── README.md
│
├── workflow/
│   └── prediction-market-agent.json
│
├── documentation/
│   └── Research & Technical Documentation.pdf
│
└── project-overview/
    └── AI Prediction Market Intelligence Agent — Project Overview.pdf

Author

Michelle Daka

AI / Data / Automation Portfolio Project

Note

This project is an educational and portfolio prototype demonstrating the use of prediction-market data and AI workflow automation. Market probabilities are signals derived from market data and should not be interpreted as guaranteed forecasts or financial advice.



