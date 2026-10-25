# Requirements Specification: FloodWise MY
Version: 1.0

## Section 0: Project Framing and Application Scope Checkpoint

| Element | Details |
| --- | --- |
| **Problem Evidence** | **1.** NADMA reported 37,328 evacuees across nine states as of 6am on 29 November 2025, with 327 temporary relief centres open (New Straits Times, 2025: https://www.nst.com.my/amp/news/nation/2025/11/1325516/flood-situation-worsens-nationwide-evacuees-rise-more-37000). This shows that floods affect many households at the same time and they need clear information quickly.<br>**2.** On 1 December 2025, JKM reported that among 14,040 evacuees still in relief centres, 2,101 were senior citizens and 4,745 were children, including 375 infants (The Vibes, 2025: https://www.thevibes.com/articles/news/116328/over-14000-flood-evacuees-remain-in-relief-centres-across-eight-states). Many affected people need simple and easy-to-follow guidance. |
| **Existing Approaches** | **1. DAISY (He et al., 2026):** a disaster information chatbot that compares a fine-tuned GPT-3.5 Turbo model with a Zephyr-7B RAG system. It uses real-time data from GDACS, and the RAG version gives source citations.<br>**2. Clara (American Red Cross):** a disaster response chatbot that gives emergency response guidelines.<br>**3. Xu et al. (2025):** a review of large language model applications in disaster management. It shows how LLMs are being used for disaster tasks, and it is a review, not a working tool. |
| **Project / Engineering Gap** | The related work shows that RAG chatbots can give cited disaster information, but I did not find one built on Malaysian flood sources. DAISY was also evaluated with only five questions per disaster type. The remaining limitation is practical: a flood assistant must stay grounded in approved Malaysian documents, show where each answer comes from, show how recent the data is, and refuse when it does not have evidence. This project is not claiming a new research method. |
| **Objective O1** | To define the scope and requirements of an assistant that answers Malaysian flood preparedness questions using only official documents. |
| **Objective O2** | To build a retrieval-based prototype that cites its sources, checks whether each citation supports the answer, and declines to answer when it finds no evidence. |
| **Objective O3** | To test the prototype on a set of 30 to 50 flood questions and measure how correct, well cited and fast its answers are, and how often it makes unsupported claims. |
| **Intended Users** | Households in flood-prone areas of Malaysia, community leaders, and relief centre volunteers. Elderly users and caregivers of young children are a priority group. |
| **Supported Tasks** | Answer flood preparedness questions (before, during and after a flood); explain flood terms and warning levels as described in the approved documents; show the source and data date for each answer; refuse out-of-scope questions and redirect users to official channels. |
| **Information / Knowledge Domain** | Flood preparedness and safety information from approved public documents (for example the ReliefWeb flood pages listed in Week 1 and official agency documents added after the Week 2 source audit). |
| **Operating Context** | A web chat interface used on a phone or laptop with an internet connection, by people preparing for or recovering from a flood. It is an information tool and not an emergency service. |
| **Important Constraints** | Only publicly accessible documents are used and licensing must be checked. The data is a snapshot, so it is not real time. Student project with a small budget and about 15 weeks. Evaluation uses a small manually checked question set. |
| **Explicit Out-of-Scope Tasks** | Predicting floods or river levels; reporting the current flood status of a specific location; telling a user whether to evacuate or whether an area is safe; replacing emergency services (users are told to call 999 and follow NADMA and local authority orders); medical, legal or insurance advice; storing personal data. |
| **Evaluation Focus** | Does FloodWise MY give correct answers that are supported by the cited source for supported flood questions, refuse out-of-scope questions, respond fast enough, and work equally well in English and Bahasa Malaysia? |

## Section 1: Functional Requirements

| # | Requirement: The system SHALL | Scope Boundary: SHALL NOT (where relevant) |
| --- | --- | --- |
| **FR1** | Answer questions about flood preparedness before, during and after a flood using the approved documents. | Invent contact numbers, procedures or facts that are not in the source documents. |
| **FR2** | Explain flood-related terms and warning levels as they are described in the approved documents. | Predict a flood or state the current flood level or status of a specific place. |
| **FR3** | Provide a source reference (document name and section or page) and the "data last checked" date for every answer based on documents. | Present unsupported information as if it came from an approved source. |
| **FR4** | State clearly when it cannot find enough evidence in the approved documents, and direct the user to official channels (NADMA, local authority, or 999 in an emergency). | Guess an answer when evidence is missing. |
| **FR5** | Decline questions outside the defined scope and redirect the user to a suitable official source. | Tell a user whether to evacuate or whether an area is safe, or give medical, legal or insurance advice. |
| **FR6** | Accept questions in English and Bahasa Malaysia and reply in the language of the question, using short and simple wording. | Switch language without the user asking. |

## Section 2: Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
| --- | --- | --- | --- | --- | --- |
| **NFR1** | Performance | The system shall keep interactive responses within the agreed latency target. | Measure end-to-end response time on the benchmark question set. | p90 latency | < 5,000 ms |
| **NFR2** | Correctness | Answers shall be factually consistent with the approved source documents. | Score the benchmark set (30 to 50 questions) with a simple rubric against the documents. | Correct response rate (%) | ≥ 85% |
| **NFR3** | Cost | Running cost shall stay within the student project budget. | Record model and API usage over the benchmark workload. | Cost per 100 queries | < US$0.50 (illustrative) |
| **NFR4** | Maintainability | Newly approved documents shall be added without rebuilding unrelated components. | Run a controlled test that adds one new document. | Time to make the new document searchable | < 15 min |
| **NFR5** | Reliability | The system shall return an answer or a clear error message for every query. | Run the benchmark set and the out-of-scope set and count failed requests. | Failed or empty responses (%) | ≤ 2% |

## Section 3: Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
| --- | --- | --- | --- | --- | --- |
| **RAI1** | Hallucination or missed information | The system shall make only claims that are supported by the retrieved source text, and abstain when no support is found. | Check each claim in benchmark answers against the cited source. | Unsupported-claim rate (%) | ≤ 5% |
| **RAI2** | Outdated data | Every document-based answer shall show the data last checked date and a link to the live official portal. | Inspect all benchmark answers for the date and link. | Date and link presence rate | 100% |
| **RAI3** | Over-reliance and out-of-scope use | The system shall refuse out-of-scope queries and show a notice to follow official instructions on safety answers. | Run a test set with at least 20 out-of-scope queries and inspect safety answers for the notice. | Refusal-and-redirect success rate; notice presence rate | ≥ 95%; 100% |
| **RAI4** | Unequal access (language) | Answer quality shall not differ beyond the agreed tolerance between English and Bahasa Malaysia questions. | Run the same benchmark questions in both languages and compare correctness. | Absolute correct response rate gap between languages | ≤ 10 percentage points |
| **RAI5** | Privacy | The system shall not ask for, store or send personal details such as name, address or exact location. | Run privacy test cases and inspect logs and outgoing model or API requests. | Number of personal data fields collected or sent | 0 |

*Note: all thresholds are proposed starting values and may be adjusted after the Week 5 model testing and the first evaluation run.*
