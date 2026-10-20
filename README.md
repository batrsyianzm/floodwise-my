# FloodWise MY: A Source-Cited Assistant for Malaysian Flood Preparedness Information

> FloodWise MY is a document-grounded question-answering assistant that helps Malaysian households find flood preparedness and evacuation information. It answers only from official documents, shows its sources and the date of the data, and declines to answer when the evidence is not enough.

## Problem Statement

During floods, Malaysian households have to check information from different places: river level readings, weather warnings, relief centre lists and safety advice. Each comes from a different agency and is updated at a different speed. Reading all of this under time pressure is slow, and people can miss important details. The people most affected are families in flood-prone states, elderly residents and parents with young children.

An AI-based solution is suitable because people ask questions in free-form, mixed Malay and English. The answer often has to be put together from several documents. FloodWise MY retrieves the relevant passages from official documents and writes an answer that is limited to those passages, with a citation for each claim. It aims to reduce unsupported claims and to measure how often they still happen. It gives information only and does not replace instructions from NADMA or local authorities.

## Evidence of the Problem

- **Evidence 1: Floods affect large numbers of people across many states.** NADMA reported 37,328 evacuees across nine states on 29 November 2025, with 327 temporary relief centres open.
  *Source:* New Straits Times (2025), *Flood situation worsens nationwide, evacuees rise to more than 37,000*. <https://www.nst.com.my/amp/news/nation/2025/11/1325516/flood-situation-worsens-nationwide-evacuees-rise-more-37000>
- **Evidence 2: Many evacuees are vulnerable groups.** JKM figures for 1 December 2025 showed that 4,745 children (including 375 infants) and 2,101 senior citizens were among those still in relief centres.
  *Source:* The Vibes (2025), *Over 14,000 flood evacuees remain in relief centres across eight states*. <https://www.thevibes.com/articles/news/116328/over-14000-flood-evacuees-remain-in-relief-centres-across-eight-states>
- **Evidence 3: Conditions change quickly, so information goes out of date.** In late November 2025, heavy rain was forecast to continue over Peninsular Malaysia for the next 96 hours while evacuations were already happening.
  *Source:* ECHO Daily Flash of 27 November 2025, on ReliefWeb. <https://reliefweb.int/report/malaysia/malaysia-floods-update-ifrc-dg-echo-met-malaysia-echo-daily-flash-27-november-2025>

## Initial Related Work / Existing Approaches

1. **DAISY: disaster information chatbot (He et al., 2026).**
   *Source:* <https://nhsjs.com/2026/daisy-a-comparative-evaluation-of-fine-tuned-gpt-3-5-and-zephyr-rag-for-natural-disaster-information-dissemination/>
   *Key limitation:* It compares a fine-tuned GPT-3.5 model with a Zephyr RAG system for disasters around the world. It is not built around Malaysian agencies, flood warning levels or local languages.
2. **Improved Flood Management and Risk Communication Through Large Language Models (Fraunhofer).**
   *Source:* <https://publica.fraunhofer.de/handle/publica/499744>
   *Key limitation:* It reports that RAG reduced factual inconsistency compared with text-only LLMs, but it proposes a multimodal framework with a flood knowledge graph that needs many data sources. It is not designed for Malaysian households.
3. **FEMA disaster chatbot using RAG (Towards Data Science article).**
   *Source:* <https://towardsdatascience.com/researching-a-multilingual-fema-disaster-bot-using-langchain-and-gpt-4-4591f26d8dcd>
   *Key limitation:* The author notes that semantic search can leave out critical information from answers. It uses US documents only and is an experiment, not a tested system.

## Engineering Gap / Limitation

Existing work shows that RAG can reduce hallucination in disaster settings, but I did not find a small, reproducible system built on Malaysian flood sources. The unresolved problem is that retrieval can miss safety-critical information and flood data goes out of date quickly. This project addresses this with citations for each claim, abstention when evidence is weak, a visible "data last checked" date, and evaluation on a manually verified question set.

## Project Objectives

- **O1:** To design a document-grounded assistant for Malaysian flood preparedness information based on official documents.
- **O2:** To develop the assistant using retrieval-augmented generation, citation checking, and guardrails that make it abstain when evidence is insufficient.
- **O3:** To evaluate the assistant using 30 to 50 benchmark questions, measuring retrieval relevance, information accuracy, citation correctness, unsupported-claim rate, and response latency.

## Target Users

Households in flood-prone areas of Malaysia, community leaders, and relief centre volunteers. Elderly users and caregivers of young children are a priority group.

## Data Sources

- **Source 1:** ReliefWeb, Malaysia Floods Nov 2025 <https://reliefweb.int/disaster/FL-2025-000211-MYS>
- **Source 2:** ReliefWeb, Malaysia Floods Nov 2024 <https://reliefweb.int/disaster/fl-2024-000218-mys>
- **Source 3:** Public InfoBanjir (JPS) <https://publicinfobanjir.water.gov.my> *(to be finalised during Week 2 source audit)*
- **Source 4:** MetMalaysia warnings <https://www.met.gov.my> *(to be finalised during Week 2 source audit)*

*Note:* Only source URLs, metadata, and dates checked are stored in this repository. Official documents are not redistributed.

## Comparison with EduAdvisor

*Similar:* Both are AI advisors for one specific user group. Both answer questions from a fixed set of documents, use a RAG-style design, and give users a simple chat interface.

*Different:* EduAdvisor supports an education setting, where a wrong answer has a lower cost. FloodWise MY supports a safety-critical setting, so it needs stricter citations and abstention rules. It also depends on fast-changing data, so every answer must show the data date, and it must work in both English and Bahasa Malaysia.

*(Adjust this section to match the actual EduAdvisor example from the course.)*

## Responsible AI Risks

1. **Hallucination or missed information.** A wrong evacuation or safety tip could put people in danger. *Mitigation:* answer only from retrieved sources, cite them, and abstain when no source supports an answer.
2. **Outdated data.** Old flood levels or warnings could be mistaken for current ones. *Mitigation:* show a timestamp on every answer and link users to the live official portals.
3. **Over-reliance.** Users may trust the chatbot instead of official instructions. *Mitigation:* show a clear notice to follow NADMA and local authority orders.
4. **Unequal access.** Elderly, rural, or Bahasa Malaysia speakers may find it hard to use. *Mitigation:* use simple wording and support both languages.

## Weekly Progress

The plan follows the SAIA 3113 AI Engineering course schedule (Weeks 1 to 15). Status is updated each week as deliverables are committed.

| Week | Course Topic | FloodWise MY Focus | Planned Deliverable | Status |
| ---- | ------------ | ------------------ | ------------------- | ------ |
| W1 | T1: AI Engineering Landscape | System map and project scope | README and docs (project scope, system map) | In progress |