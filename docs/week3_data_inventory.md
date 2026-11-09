# FloodWise MY

A document-grounded flood preparedness assistant for Malaysia, built as a source-cited RAG pipeline.

> **Project limitation:** FloodWise MY provides information only. It does not replace official instructions from NADMA or local authorities.

## Contents

- [Objectives](#objectives)
- [1. Required Data](#1-required-data)
- [2. Source Records](#2-source-records)
- [3. Inspection Evidence](#3-inspection-evidence)
- [4. Open Items](#4-open-items)
- [Part A: Data Preparation Plan](#part-a-data-preparation-plan)

## Objectives

O1, O2 and O3 are the project objective IDs used in this README. Exact FR, NFR and RAI IDs should be added only after checking the Topic 2 requirements document.

- **O1:** Document-grounded flood preparedness assistant
- **O2:** Source citations, evidence-grounded answers and abstention when evidence is insufficient
- **O3:** Evaluation of answer quality and retrieval

---

## 1. Required Data

| Requirement / Objective | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| O1: Document-grounded flood preparedness assistant | Malaysian flood reports, preparedness information, evacuation information and disaster updates | ReliefWeb: Malaysia Floods Nov 2025 and Malaysia Floods Nov 2024 | Provides Malaysia-specific disaster reports and flood situation information. Reports are historical and must not be treated as live flood conditions. |
| O1, O2: Retrieve relevant flood information | Flood situations, river levels and related flood-monitoring information | [Public InfoBanjir (JPS)](https://publicinfobanjir.water.gov.my/) | Relevant to Malaysian river and flood monitoring. Source collection and access method are still to be finalised during the source audit. |
| O1, O2: Retrieve weather warning information | Weather warnings and continuous rain information | [MetMalaysia](https://www.met.gov.my/) | Provides official Malaysian weather information. Warning information can change, so its issue and update dates must be retained. Source selection is still to be finalised during the source audit. |
| O2: Source citations and evidence-grounded answers | Source URL, document title, issuing organisation, publication/update date and supporting passages | ReliefWeb, JPS and MetMalaysia | Supports source traceability. Each answer should include citations linked to the retrieved evidence. |
| O2: Abstention when evidence is insufficient | Relevant retrieved passages and evidence for determining whether a question can be answered | Selected official flood information sources | Supports the system's ability to decline unsupported questions rather than generate unverified information. |
| O3: Evaluation of answer quality and retrieval | 30 to 50 manually verified benchmark questions with expected answers and supporting sources | Project-created evaluation dataset based on selected flood sources | Allows evaluation of retrieval relevance, information accuracy, citation correctness, unsupported-claim rate and response latency. Questions and reference answers must be manually checked. |
| Responsible AI: Information freshness and provenance | Source identity, source URL, publication/update date and date last checked | ReliefWeb, Public InfoBanjir and MetMalaysia | Helps prevent outdated information from being presented as current. The assistant should display the data date and direct users to official portals for live updates. |

---

## 2. Source Records

| Field | S01: ReliefWeb Malaysia Floods Nov 2025 | S02: ReliefWeb Malaysia Floods Nov 2024 | S03: Public InfoBanjir (JPS) | S04: MetMalaysia |
|---|---|---|---|---|
| **Source identity** | ReliefWeb | ReliefWeb | Department of Irrigation and Drainage Malaysia (JPS) | Malaysian Meteorological Department |
| **URL** | https://reliefweb.int/disaster/FL-2025-000211-MYS | https://reliefweb.int/disaster/fl-2024-000218-mys | https://publicinfobanjir.water.gov.my/ | https://www.met.gov.my/ |
| **Data type** | Online disaster reports and related information | Online disaster reports and related information | Online flood and river monitoring information | Online weather warnings and meteorological information |
| **Purpose** | Information about the November 2025 Malaysia flood situation | Information about the November 2024 Malaysia flood situation | Flood-monitoring and river-level information | Weather warnings and continuous rain information |
| **Version / date** | Malaysia Floods Nov 2025. Individual report publication dates must be recorded. | Malaysia Floods Nov 2024. Individual report publication dates must be recorded. | Dynamic information. Record the date and time accessed. | Dynamic warning information. Record the warning issue/update date. |
| **Access rights** | Publicly accessible website. Check individual report reuse conditions. | Publicly accessible website. Check individual report reuse conditions. | Publicly accessible website. Check applicable access and reuse conditions. | Publicly accessible website. Check applicable access and reuse conditions. |
| **Collection status** | Source identified in this README. Local content extraction not yet confirmed. | Source identified in this README. Local content extraction not yet confirmed. | To be finalised during Week 2 source audit. | To be finalised during Week 2 source audit. |
| **Local file details** | Not stored as a redistributed official document in the repository. | Not stored as a redistributed official document in the repository. | To confirm after source audit. | To confirm after source audit. |

### Data storage approach

The GitHub repository currently stores source URLs, metadata and checked dates. Official source documents are not redistributed.

The collected information should retain its source identity and date so that the system can provide traceable answers.

---

## 3. Inspection Evidence

### Inspection 1: ReliefWeb Malaysia Floods Nov 2025

| # | Observation | Evidence / source information | Implication |
|---|---|---|---|
| 1 | The source is specific to Malaysian flood events. | ReliefWeb Malaysia Floods Nov 2025. | Relevant to the Malaysian flood preparedness domain. |
| 2 | The information relates to a particular disaster period. | Source URL: https://reliefweb.int/disaster/FL-2025-000211-MYS | Retain the publication date and disaster period to avoid treating historical information as current. |
| 3 | The source can support disaster-related questions. | The source is included in the FloodWise MY data source list. | Relevant reports can be selected for the document-grounded knowledge base. |
| 4 | Information may become outdated. | The report covers a specific flood event. | Display the source date and avoid presenting historical reports as live flood status. |

### Inspection 2: ReliefWeb Malaysia Floods Nov 2024

| # | Observation | Evidence / source information | Implication |
|---|---|---|---|
| 1 | The source provides information about an earlier Malaysian flood event. | ReliefWeb Malaysia Floods Nov 2024. | Can provide historical disaster context. |
| 2 | It covers a different period from the November 2025 source. | Source URL: https://reliefweb.int/disaster/fl-2024-000218-mys | The two sources can provide information from different flood events. |
| 3 | Historical information may not reflect current conditions. | The source refers to the November 2024 flood event. | The assistant must distinguish historical reports from current flood information. |

### Inspection 3: Public InfoBanjir (JPS)

| # | Observation | Evidence / source information | Implication |
|---|---|---|---|
| 1 | The source is identified in the project data source list. | https://publicinfobanjir.water.gov.my/ | Relevant candidate for Malaysian flood and river monitoring information. |
| 2 | The source provides information that may change over time. | Public flood-monitoring portal. | The retrieval or access date should be recorded. |
| 3 | The source audit is not yet finalised. | To be finalised during Week 2 source audit. | Confirm the exact data fields, access method and collection format before ingestion. |

### Inspection 4: MetMalaysia

| # | Observation | Evidence / source information | Implication |
|---|---|---|---|
| 1 | The source is identified in the project data source list. | https://www.met.gov.my/ | Relevant candidate for official Malaysian weather warnings. |
| 2 | Weather warnings can change over time. | Official meteorological warning information. | The warning issue/update date must be retained. |
| 3 | The source audit is not yet finalised. | To be finalised during Week 2 source audit. | Confirm the relevant warning categories, access method and collection format before ingestion. |

> **Inspection limitation:** The observations above describe the sources identified in this README. They do not confirm that local documents have been downloaded, parsed or successfully ingested. Actual extraction evidence, such as text samples, page counts or API responses, must be recorded after inspection in Google Colab.

---

## 4. Open Items

- [ ] Finalise the Public InfoBanjir (JPS) source audit.
- [ ] Finalise the MetMalaysia warning source audit.
- [ ] Confirm the exact data access and collection methods for JPS and MetMalaysia.
- [ ] Record the publication dates and retrieval dates for the selected ReliefWeb reports.
- [ ] Confirm the source reuse conditions and permissions before processing or redistributing content.
- [ ] Prepare the approved source data in an appropriate format for the Colab pipeline.
- [ ] Inspect the actual collected source data and record representative text samples or API responses.
- [ ] Record the actual file size, page count and SHA-256 checksum for any locally collected files.
- [ ] Create a source metadata record containing the source title, URL, organisation and date last checked.
- [ ] Prepare the 30 to 50 manually verified benchmark questions for O3.
- [ ] Verify that each benchmark answer has supporting evidence and a source reference.
- [ ] Ensure that historical flood reports are not presented as current flood conditions.

---

## Part A: Data Preparation Plan

### A2. Google Colab Workspace

Notebook: `week3_pipeline.ipynb`

Workspace structure:

```text
week3_project/
├── data/
│   ├── raw/
│   └── processed/
└── notebooks/
    └── week3_pipeline.ipynb
```

The raw folder will contain authorised collected source files or exports. The processed folder will contain extracted and prepared data.

### A3. Data Inspection

The selected data may include online reports, PDF/TXT documents and time-sensitive source information.

The inspection process will check:

- Source identity and URL.
- Publication or update date.
- File type or response format.
- Extracted text or returned data fields.
- Missing information and extraction limitations.
- Whether the information is relevant to Malaysian flood preparedness.

### A4. Data Inventory

This inventory records the selected sources, their intended purpose, access status and inspection evidence.

The original source information and metadata should be retained to support the source-cited RAG pipeline.
