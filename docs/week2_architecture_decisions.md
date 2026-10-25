# V1 Architecture, Decisions and Traceability — UTM GenAI Policy Assistant

Diagram: `docs/week2_architecture_v1.png`

## Step C1 — Simplest architecture
User Query → AI Application → AI Capability (language model) → Response

This simple flow cannot meet several requirements. A language model alone has no access to the UTM documents (FR1, FR3, NFR2), cannot show verifiable sources (RAI4), and needs controls for scope, abstention and privacy (FR5, FR6, RAI1, RAI5, RAI6).

## Step C2 — Architecture needs from requirements

| Requirement ID | What does the requirement need? | Architecture Response |
|---|---|---|
| FR1 / FR2 / NFR2 | Answers must be based on the two UTM documents and reach the correctness threshold. | Knowledge Source + Knowledge Access component |
| FR3 / RAI4 | Every answer must show the document, edition/date and section. | Source tracking in Knowledge Access, checked by an Output Grounding & Citation Check |
| FR4 / RAI2 | Assessment-specific questions must lead to a lecturer referral, not a ruling. | Input Scope Control (detects assessment-specific and misconduct-ruling queries) and Output Check |
| FR5 / RAI1 | The system must say "not found" when evidence is missing and avoid unsupported statements. | Output Grounding & Citation Check with abstention |
| FR6 / RAI5 | Out-of-scope queries must be refused and redirected. | Input Scope & Privacy Control |
| RAI6 | No personal data sent to external model APIs. | Privacy filter inside the input control, before the model |
| FR7 / RAI3 | Malay and English questions must both be handled and compared. | Language Model Service that supports both languages; Evaluation Point comparing the two |
| NFR1 / NFR4 | Latency and cost within threshold. | Short interaction path with limited retrieved passages sent to the model |
| NFR5 | Documents can be updated without rebuilding other parts. | Knowledge Source kept separate from the other components |
| Part B criteria | Evidence must be collected for evaluation. | Evaluation Point on the response path |

## Step C3 — V1 conceptual architecture
See `docs/week2_architecture_v1.png`. Components are labelled by function. No specific model, database, or framework is chosen yet.

Flow: Student → Application Interface → Input Scope & Privacy Control → Knowledge Access (reads Knowledge Source) → Language Model Service → Output Grounding & Citation Check → Response with sources or lecturer referral → Application Interface. Out-of-scope queries leave the Input Control as a refusal and redirect. An Evaluation Point is applied on the response path.

## Step C4 — Architecture Decision Records

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| ADR1: Include knowledge access to the two UTM documents | FR1, FR2, FR3, RAI4, NFR2: answers must be grounded in official documents and show their source. | (a) Language model only. (b) Static keyword search of the documents. (c) Dynamic knowledge access that finds relevant passages and gives them to the model. | Dynamic knowledge access (generic component) | More system complexity and more steps to test, but better grounding and traceable sources. Wrong passages may still be retrieved, so retrieval must be evaluated. |
| ADR2: Include an explicit input scope and privacy control | FR4, FR6, RAI2, RAI5, RAI6: assessment-specific, misconduct-ruling and out-of-scope queries must not be answered, and personal data must not leave the system. | (a) Instructions in the model prompt only. (b) Explicit scope and privacy control before the model. | Explicit control before the model | Possible false refusals and extra processing time (affects NFR1), but more reliable and testable refusal behaviour. |
| ADR3: Include an output grounding and citation check | FR3, FR5, RAI1, RAI4: answers must be supported by the cited text and show the source, or say "not found". | (a) Trust the model's answer. (b) Ask the model to cite in its prompt only. (c) A separate check of the draft answer against the retrieved passages. | Separate output check | Extra latency and cost per query (NFR1, NFR4) in exchange for fewer unsupported statements. |
| ADR4: Keep the Knowledge Source separate from the other components | NFR5: updated UTM documents must be added without rebuilding unrelated parts; constraint: PDFs stay local. | (a) Build documents into the model or prompt. (b) Separate, replaceable knowledge source. | Separate knowledge source with document edition/date recorded | Needs a defined update process and version records, but updates are quick and sources can show which edition was used. |

## Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR1, FR2, FR3, RAI4 | Source citation and support | Knowledge Source, Knowledge Access, Output Grounding & Citation Check | Citation presence and support check on test answers |
| O1 / O3 | FR1, FR2, NFR2 | Answer correctness | Knowledge Access, Language Model Service | Correct response rate on 40-question set |
| O2 | FR4, RAI2 | Lecturer referral for assessment-specific questions | Input Scope & Privacy Control, Output Check | Referral rate on 15 assessment-specific questions |
| O2 | FR5, RAI1 | Abstention when evidence is missing | Output Grounding & Citation Check | Abstention rate on 15 unanswerable questions |
| O2 | FR6, RAI5, RAI6 | Out-of-scope refusal | Input Scope & Privacy Control | Refusal rate on 20 out-of-scope queries; payload inspection |
| O3 | FR7, RAI3 | Language fairness and clarity | Language Model Service, Evaluation Point | English vs Malay correctness gap |
| O3 | NFR1, NFR4 | Response latency; cost per query | Whole response path (kept short) | p90 latency and cost per 100 queries on test workload |

## Final review checklist
- Each major component supports at least one requirement or evaluation need (see Step C2 and the traceability matrix).
- Knowledge Source, safety controls and an Evaluation Point are all shown.
- No specific technology has been chosen yet. Choices such as the model, retrieval method and framework are left for later topics.
- A simpler design (model only) was considered in ADR1 and ADR3 but cannot meet the grounding, citation and abstention requirements.

---

## Reflection (Part C)

**Which architecture decision involved the most important trade-off? Which requirement drove the decision, and what trade-off did you accept?**

The most important trade-off was **ADR2, the explicit input scope and privacy control**. It was driven mainly by FR4 and RAI2 (do not say AI is allowed or forbidden for a specific assessment), together with RAI5 (refuse out-of-scope queries) and RAI6 (no personal data sent to external model APIs).

The alternative was to put these rules only in the model's instructions, which is simpler. I chose a separate control before the model because it is easier to test and more reliable. The trade-off I accepted is that the control may refuse some valid questions (false refusals), such as a general question that sounds like an assessment-specific one. It also adds processing time, which affects the latency requirement (NFR1). I accepted this because for a policy assistant, a wrongly refused question is less harmful than a student receiving false permission to use AI. I will measure the false refusal rate in the later evaluation and adjust the control if it is too strict.
