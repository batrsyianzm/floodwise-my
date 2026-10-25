# Architecture Decisions and Traceability: FloodWise MY

## Architecture Needs from Requirements

| Requirement ID | What does the requirement need? | Architecture Response |
| --- | --- | --- |
| FR1 / FR2 / NFR2 | Answers must come from approved flood documents and be correct. | Knowledge source and knowledge access component |
| FR3 / RAI1 | Every claim needs a source, and unsupported claims must be blocked. | Source tracking and a citation and grounding check |
| FR4 / RAI1 | The system must abstain when evidence is missing. | Abstain path inside the grounding check |
| FR5 / RAI3 | Out-of-scope queries must be refused and redirected. | Scope and safety control before generation |
| RAI2 | Answers must show the data date and live portal link. | Document metadata (date checked, source link) added to every response |
| FR6 / RAI4 | Questions must work in English and Bahasa Malaysia. | Language Model Service that handles both languages, and an interface that shows the answer in the user's language |
| NFR1 | Responses must meet the latency threshold. | Keep the flow short with only the components that are needed |
| RAI5 | No personal data may be collected. | Interface with no personal data fields, and no query logging of personal details |
| All evaluated requirements | Results must be measured against the benchmark. | Evaluation point that compares outputs with the benchmark question set |

## Architecture Decision Record (ADR)

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
| --- | --- | --- | --- | --- |
| Include knowledge access | FR1, FR3, NFR2: answers must be grounded in approved flood documents. | Base model only; static keyword search; dynamic knowledge access | Generic knowledge access component over the approved documents | More system complexity, and retrieval can miss relevant passages, but answers are better grounded and can be traced to a source. |
| Include explicit scope control before generation | FR5, RAI3: out-of-scope queries must be refused. | Instruction in the prompt only; explicit scope control component | Explicit scope control | Possible false refusals and a little extra processing time, but safer behaviour for evacuation and safety questions. |
| Include a citation and grounding check before the answer is shown | FR3, FR4, RAI1: unsupported claims must be blocked and the system must abstain when evidence is missing. | Trust the model to cite correctly; check only a sample during evaluation; check every answer | Check every answer before it is shown | Higher latency (pressure on NFR1) and a risk of blocking some correct answers, but fewer unsupported claims reach users. |
| Use a document snapshot with a "data last checked" date instead of live data feeds | RAI2, NFR3, constraint: small budget and time, public documents only. | Connect live river level and warning feeds; document snapshot with live portal links | Document snapshot with a date and links to live portals | Information may be out of date, but the system is simpler, cheaper and easier to evaluate. Users are sent to the live official portals for current conditions. |

## Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
| --- | --- | --- | --- | --- |
| O1 | FR1, FR3 | Citation and source support | Knowledge source, knowledge access, source tracking | Citation presence and match results from the Week 10 benchmark run |
| O2 | RAI1, FR4 | Citation and source support; abstention | Citation and grounding check with abstain path | Unsupported-claim rate and abstention test results |
| O2 | FR5, RAI3 | Out-of-scope refusal and redirect | Scope and safety control | Result of the 20+ out-of-scope query test |
| O3 | NFR2 | Answer correctness | Knowledge access and Language Model Service | Correct response rate on the 30 to 50 question benchmark |
| O3 | NFR1 | Response latency | Whole request path | p90 latency from the benchmark run |
| O3 | FR6, RAI4 | Language parity and clarity | Language Model Service and interface | English versus Bahasa Malaysia correctness gap |

## Final Review Checklist

- **Does each major component support a requirement or evaluation need?** Yes. Each component in the diagram is linked to a requirement in the table above.
- **Is any important data source, safety control, or evaluation point missing?** The approved document list still needs the Week 2 source audit. The evaluation point is included.
- **Have I added any technology or component that is not yet justified?** No. Components are named by function, and no specific model, database or framework is chosen yet.
- **Is there a simpler architecture that could still satisfy the requirements?** A base model alone would not meet FR3, FR4 or RAI1, so the knowledge access, scope control and grounding check are needed.
