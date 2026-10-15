# Climate-Smart Agri-Extension Advisory Agent 
## Project Overview 
* **Theme:** Climate-Smart Advisory 
* **Target Administrative Unit:** Sub-County / Ward Agricultural Extension Office (Saboti ward) 
* **Target Users:** Frontline Extension Officers serving smallholder farmers in household clusters. 
* **The Backlog Challenge:** Frontline extension officers are responsible for thousands of smallholders. Cross-referencing 7-day rainfall forecasts, localized pest alerts (e.g., Fall Armyworm), and shifting market aggregation prices against individual household plot calendars takes 15 minutes per cluster by hand.
## System Architecture & Workflow (n8n + Open-Source AI) Our solution turns scattered signals into structured, defensible advisories in under 5 seconds using an automated n8n workflow: 
 [ Data Signals (Weather, Pest, Market, Plots) ]
[ n8n Chat Trigger Node ]
[ Open-Source Model via Groq (allam-2-7b) ] 
[ Extension Officer Handoff (DRAFT ONLY) (AGENT STOPS HERE) ]
[ Officer Reviews & Approves) ] 
[ Manual Dispatch to Farmer (SMS/USSD) ] 

• Signals Ingested: Ingests household plot records, local meteorological bulletins, pest warnings, and market aggregation pricing.

• AI Processing: Evaluates crop growth stages against weather thresholds (e.g., rainfall wash-off risks) and active pest alerts using an open-source model running via groq.

• Draft Advisory Generation: Formats a structured draft advisory containing explicit citations for every recommendation.
Technical Standards & Hackathon Safeguards Enforced
• Human-in-the-Loop:

• The AI agent never sends automated messages, places orders, or applies for loans on behalf of farmers.

• The agent functions exclusively as a drafting assistant. It halts completely at the Officer Handoff Block with unchecked approval boxes [ ]. An extension officer must review and sign off before any message reaches a household.

• Absolute Defensibility:

• Every single recommendation cites its exact source (e.g., Kenya Meteorological Department Bulletin, County Pest Alert #04, MIS Market Index). Unsourced advice is strictly prohibited.

• Data Sovereignty & Open Weights:

• To comply with agricultural data-sovereignty principles, sensitive household plot records are processed using open-weight models (allam-2-7b) orchestrated via open-source tools (n8n).
Master System Prompt
The AI Agent node in n8n uses the following system prompt to enforce strict safety, citation, and structural guidelines:

Role: You are an AI Agricultural Extension Assistant for the Sub-County Extension Office. Your job is to analyze weather, pest, and market signals against smallholder household plot records to draft clear, defensible, climate-smart advisories. STRICT MANDATORY RULES (NON-NEGOTIABLE): 
1. HUMAN-IN-THE-LOOP & NO DISPATCH: - You MUST NOT offer, promise, or claim to send the message to the farmer yourself. - You are solely an AI Drafting Tool. The message ends at the Officer Handoff block. - Dispatch to the farmer is handled exclusively by the Extension Officer via SMS, USSD, or home visit. 
2. ABSOLUTE DEFENSIBILITY & CITATIONS: - Every claim, warning, or recommended action MUST explicitly cite the specific source provided in the inputs (Met Dept Bulletin date, Pest Alert number, MIS Price Index). 3. ADVISORY STRUCTURE: Output strictly using this exact template:
[DRAFT ADVISORY - PENDING OFFICER REVIEW] Target Household: {Farmer Name} ({Household ID}) Location: {Location} | Crop: {Crop} ({Stage}) | Plot Size: {Plot Size} 1. IDENTIFIED RISKS & OPPORTUNITIES: - Risks: [Key environmental & pest threats] - Opportunities: [Key market/price insights] 2. RECOMMENDED ACTION DRAFT: - Weather & Planting Actions: [Specific actions with dates] - Pest Management Actions: [Specific actions with timing] 3. DATA SOURCES CITED (EVIDENCE): - Weather Data: [Met Dept Bulletin date] - Pest Signal: [Pest Alert number & date] - Market Data: [MIS Price Index date] 4. OFFICER HANDOFF BLOCK (FOR OFFICER REVIEW): Status: DRAFT ONLY — AWAITING HUMAN APPROVAL [ ] APPROVED for manual dispatch by Extension Officer: 
[ ] REJECTED / NEEDS REVISION 

Sample Input Payload & Generated Output
Sample Input Payload
FARMER RECORD: Jane Nafula Wekesa,  HH-2026-081, Saboti Village, Maize (Tasseling stage), 2 Acres WEATHER & PEST SIGNALS: - Kenya Met Dept 7-Day Bulletin (Oct 14, 2026): 65mm heavy rainfall expected Oct 17-19 in Saboti. - County Pest Alert #04 (Oct 13, 2026): Fall Armyworm active in Saboti Ward. MARKET SIGNALS: - MIS Price Index (Oct 14, 2026): Kiminini Depot KSh 3,450/bag vs Kitale Market KSh 3,200/bag. 

Generated Agent Output
Plaintext
DRAFT ADVISORY - PENDING OFFICER REVIEW Target Household: Jane Nafula Wekesa (HH-2026-081) Location: Saboti Village | Crop: Maize (Tasseling stage) | Plot Size: 2 Acres 1. IDENTIFIED RISKS & OPPORTUNITIES: - Risks: Extreme heavy rainfall (65mm) expected Oct 17-19 pose a spray wash-off risk. Active Fall Armyworm threat in Saboti Ward requires immediate control before tasseling damage occurs. - Opportunities: Kiminini Depot currently offers a higher buying price (KSh 3,450/bag) compared to Kitale Market (KSh 3,200/bag). 2. RECOMMENDED ACTION DRAFT: - Weather & Spraying Actions: Apply chemical treatment before Oct 16 to avoid pesticide wash-off from heavy rains between Oct 17-19. 
Hold top-dressing fertilizer until after rain subsides to prevent leaching. 
- Pest Management Actions: Monitor plot daily for Fall Armyworm egg masses and apply recommended treatment prior to Oct 16. 
3. DATA SOURCES CITED (EVIDENCE): - Weather Data: Kenya Met Dept 7-Day Bulletin (Oct 14, 2026) - Pest Signal: County Ministry of Agriculture Field Pest Alert #04 (Oct 13, 2026) - Market Data: Regional MIS Price Index (Oct 14, 2026) 4. OFFICER HANDOFF BLOCK (FOR OFFICER REVIEW): Status: DRAFT ONLY — AWAITING HUMAN APPROVAL [ ] APPROVED for manual dispatch by Extension Officer: ___________________ [ ] REJECTED / NEEDS REVISION 
