Climate-Smart Agri-Extension Advisory Agent
Project Overview
• Theme: Climate-Smart Advisory

• Target Administrative Unit: Ward Agricultural Extension Office, Saboti Ward.

• Target Users: Frontline Extension Officers serving smallholder farmers in household clusters.

• The Backlog Challenge: Frontline extension officers are responsible for thousands of smallholders. Cross-referencing 7-day rainfall forecasts, localized pest alerts like Fall Armyworm, and shifting market aggregation prices against individual household plot calendars takes ~15 minutes per cluster by hand.

 System Architecture & Workflow (n8n + Open-Source AI)
Our solution turns scattered signals into structured, defensible advisories in under 5 seconds using an automated n8n workflow:

[ Data Signals (Weather, Pest, Market, Plots) ] │  [ n8n Chat Trigger Node ] │  [ Open-Source Model via Groq (Llama 3) ] │  [ Extension Officer Handoff (DRAFT ONLY) ] <-- AGENT STOPS HERE │ (Officer Reviews & Approves) │  [ Manual Dispatch to Farmer via (SMS/USSD) ] 
• Signals Ingested: Ingests household plot data (farmers.json), local meteorological forecasts and pest warnings (weather_pest.json), and market aggregation pricing (market_prices.json).

• AI Processing: Evaluates crop growth stages against weather thresholds (e.g, rainfall wash-off risks) and active pest alerts using an open-source model running via Groq.

• Draft Advisory Generation: Formats a structured draft advisory containing explicit citations for every recommendation.

 Mandatory Safeguards & Rules Enforced
• Human-in-the-Loop:

• The AI agent never sends automated messages, places orders, or applies for loans on behalf of farmers.

• The agent functions exclusively as a drafting assistant. It halts completely at the Officer Handoff Block with unchecked approval boxes [ ]. An extension officer must review and sign off before any message reaches a household.

• Absolute Defensibility:

• Every single recommendation cites its exact source (e.g., Kenya Meteorological Department Bulletin, County Pest Alert #04, MIS Market Index). Unsourced advice is strictly prohibited.

 Repository Structure

. ├── README.md <-- Submission documentation ├── prompts/ │ └── system_prompt.txt <-- Master system prompt used in the n8n AI Agent node └── data/ ├── farmers.json <-- Sample household plot records ├── weather_pest.json <-- Sample weather & pest alerts └── market_prices.json <-- Sample market aggregation center prices.
