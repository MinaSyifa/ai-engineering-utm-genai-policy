# Requirements Specification — UTM GenAI Policy Assistant
Version: 1.0

## Section 0 — Project Framing & Application Scope Checkpoint

| Element | Content |
|---|---|
| **Problem Evidence** | (1) UTM's guidelines say GenAI use must follow the Academic Regulations and related documents, and that lecturers must state GenAI rules for each assessment. A student therefore has to check more than one document and each course (UTM, *Guidelines for the Use of Generative AI in Teaching and Learning*). (2) Published work reports that students struggle to find their way through complex academic policies, which delays access to correct information (Torkian & Zhou, *Carolina Guide*, arXiv:2606.28360). |
| **Existing Approaches** | (1) Carolina Guide (University of South Carolina): a RAG assistant that gives citation-supported answers on academic policy and refuses personalised advising. (2) Bournemouth University RAG chatbot (Meacham et al., *AI & Society*): answers questions on one research degree code of practice, with source attribution, evaluated with RAGAS. (3) RMUTTOBot: a RAG chatbot for university admission questions that combines structured table data with retrieval. |
| **Project / Engineering Gap** | The existing assistants focus on admissions, advising or a single policy document. None of them answers "Can I use AI for this?" by combining several UTM documents, giving cited answers, and clearly saying when the answer depends on the lecturer or assessment. This is a practical limitation (grounding, traceability and safe uncertainty), not a claim of scientific novelty. |
| **Objective O1** | To design a document-grounded assistant that answers questions about generative AI use using official UTM documents. |
| **Objective O2** | To develop the assistant with retrieval, source citations, and guardrails that stop it from giving final misconduct rulings or assessment-specific permission. |
| **Objective O3** | To evaluate the assistant for answer correctness, groundedness, latency, and safe refusal behaviour. |
| **Intended Users** | UTM undergraduate students who need quick and clear guidance on acceptable use of generative AI in coursework. |
| **Supported Tasks** | (a) Answer questions about what UTM documents say on GenAI use in learning and assessment. (b) Explain academic misconduct definitions and general procedures in the Academic Regulations. (c) Show the source of each answer. (d) Tell the student when the rule depends on the lecturer or the assessment and where to check. |
| **Information / Knowledge Domain** | Two public UTM documents: the Guidelines for the Use of Generative AI in Teaching and Learning, and the Academic Regulations for Full-Time Bachelor's Degree Programmes (16th edition). |
| **Operating Context** | A web chat application used by students on laptop or phone, at any time, as a first reference before they ask a lecturer. It is not an official UTM service. |
| **Important Constraints** | Only two public documents are used in V1. The PDFs are kept local and are not redistributed . Individual course rules are not available to the system. Semester timeline and student budget limit model cost. Documents may be updated by UTM, so the system must show which document version it used. |
| **Explicit Out-of-Scope Tasks** | Deciding whether a student committed misconduct or predicting a penalty; saying that a specific assessment allows or forbids AI; legal, medical or mental health advice; writing or editing a student's assignment; policies of other universities; handling personal data or case files. |
| **Evaluation Focus** | Does the assistant give correct, source-supported answers to UTM GenAI policy questions, refer students to their lecturer when the rule is assessment-specific, refuse out-of-scope requests, and respond fast enough for routine use? |

---

## Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|---|---|
| **FR1** | Answer questions about what the UTM Guidelines for the Use of GenAI say about GenAI use in teaching, learning and assessment. | Invent rules, conditions or exceptions that are not in the source documents. |
| **FR2** | Answer questions about academic misconduct definitions and general procedures that are stated in the UTM Academic Regulations. | Decide whether a particular student committed misconduct, or predict the penalty for a specific case. |
| **FR3** | Provide a source reference (document name and section or page) with every answer based on the UTM documents. | Present unsupported information as if it came from an official document. |
| **FR4** | State clearly when the answer depends on the lecturer, course or assessment, and tell the student to check the course outline or ask the lecturer. | Say that AI use is allowed or not allowed for a specific course or assessment. |
| **FR5** | Say that it cannot find enough supporting information in the documents when the question is not answered by them. | Guess an answer to fill the gap. |
| **FR6** | Decline queries outside the defined scope (for example legal advice, other universities' policies, writing a student's assignment) and redirect the student to a suitable UTM contact or source. | Provide legal, medical or mental health advice, or produce assessed work for the student. |
| **FR7** | Accept questions in English or Malay and reply in the same language, using clear language suitable for undergraduate students. | - |

---

## Section 2 — Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | Performance | The system shall respond within an interactive time for routine questions. | Measure end-to-end latency (question sent to full answer shown) on a test set of at least 40 supported questions. | p90 latency | < 5,000 ms |
| **NFR2** | Correctness | Answers shall be factually consistent with the UTM source documents. | Score a curated set of at least 40 supported questions against a reference answer with a defined rubric (correct / partly correct / incorrect). | Correct response rate (%) | ≥ 85% |
| **NFR3** | Reliability | The system shall give consistent answers to the same question. | Ask each of 20 questions 3 times and compare whether the main answer stays the same. | Answer agreement across repeated runs (%) | ≥ 90% |
| **NFR4** | Cost | Model usage cost shall stay within the student project budget. | Record model/API usage during the evaluation workload. | Cost per 100 queries | < US$1.00 per 100 queries (illustrative) |
| **NFR5** | Maintainability | An updated UTM document shall be added without rebuilding unrelated components. | Run a controlled document-update test: replace one document and check answers reflect the change. | Update time | < 15 min |

---

## Section 3 — Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | Hallucination | Statements in answers shall be supported by the cited UTM source, and the system shall abstain when evidence is missing. | (a) Check each statement in 40 answers against its cited passage. (b) Run 15 questions whose answers are not in the documents. | (a) Supported-statement rate (b) Correct abstention rate | (a) ≥ 95% (b) ≥ 90% |
| **RAI2** | Over-confidence on assessment-specific rules | The system shall not state that AI is allowed or forbidden for a specific assessment, and shall refer the student to the lecturer. | Run a test set of at least 15 questions about specific courses or assignments (e.g. "Can I use ChatGPT for my SAIA assignment?"). | Correct lecturer-referral rate (%) | ≥ 95% |
| **RAI3** | Fairness / language | Answer quality shall not differ beyond the agreed tolerance between English and Malay questions. | Translate a set of 30 test questions into Malay and compare correctness rates for both languages. | Absolute correctness gap between languages | ≤ 10 percentage points |
| **RAI4** | Transparency / outdated documents | Every document-based answer shall show the source document and its edition or date, so students can check if it is current. | Inspect document-based answers for presence of document name, edition/date and section. | Source-and-version presence rate | 100% |
| **RAI5** | Scope / safety | The system shall refuse out-of-scope queries (including requests for misconduct rulings or assignment writing) and redirect appropriately. | Run a defined scope test set of at least 20 out-of-scope queries. | Refusal-and-redirect success rate | ≥ 95% |
| **RAI6** | Privacy | The system shall not send personal data (names, matric numbers, case details) to external model APIs. | Run privacy test cases containing fake personal data and inspect outgoing payloads. | Number of detected personal-data transmissions | 0 |

---

## Reflection (Part A)

**Which Responsible AI requirement in your project will be the most difficult to measure or enforce technically? Why?**

The most difficult one will be RAI2, which says the system must not state that AI is allowed or forbidden for a specific assessment. It is hard to enforce because the system first has to notice that a question depends on a lecturer's rule. Students will not always ask directly. A question such as "Can I use AI to fix the grammar in my report?" does not mention a course, but the answer still depends on the assessment. If the system answers with the general UTM guideline, it may sound like permission, even though the lecturer may not allow it.

It is also hard to measure. I need to write a test set that covers many direct and indirect ways of asking, and I may miss some of them, so a high referral rate on my test set may not match real use. Checking whether an answer sounds like permission also needs human judgement, not only an automatic check. For these reasons I plan to combine a scope control with an output check and to review the failed test cases by hand.
