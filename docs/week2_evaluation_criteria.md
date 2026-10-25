# Evaluation Criteria — UTM GenAI Policy Assistant

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O3 / NFR2 / FR1 / FR2 | Answer correctness | Domain-specific capability | Score a curated set of at least 40 supported questions on UTM GenAI and misconduct policy against reference answers using a rubric. | Correct response rate (%) | ≥ 85% | Students receive reliable answers for routine policy questions and avoid accidental rule breaking. |
| O1 / FR3 / RAI1 / RAI4 | Source citation and support | Domain-specific capability | Inspect answers for source name, edition/date and section, and check whether each statement is supported by the cited passage. | Source-and-version presence rate; supported-statement rate | 100% presence; ≥ 95% supported | Students can verify every answer in the official document and see if it is current. |
| O2 / FR4 / RAI2 | Lecturer referral for assessment-specific questions | Instruction-following capability | Test with at least 15 questions about specific courses or assignments. | Correct lecturer-referral rate (%) | ≥ 95% | Students are not misled about their own assessments, which lowers the risk of integrity problems. |
| O2 / FR5 / RAI1 | Abstention when evidence is missing | Instruction-following capability | Test with 15 questions that are not answered by the two documents. | Correct abstention rate (%) | ≥ 90% | Students are told "not found" instead of receiving an invented rule. |
| O2 / FR6 / RAI5 | Out-of-scope refusal | Instruction-following capability | Run a defined scope test set of at least 20 out-of-scope queries (misconduct rulings, assignment writing, legal advice). | Refusal-and-redirect success rate (%) | ≥ 95% | The system stays within its safe role and sends students to the right UTM contact. |
| O3 / FR7 / RAI3 | Language fairness and clarity | Generation capability | Compare correctness on 30 questions asked in English and in Malay; rate clarity with a simple 3-point rubric. | Correctness gap between languages (percentage points); clarity score | Gap ≤ 10 pp; average clarity ≥ 2 out of 3 | Students who ask in Malay get equally useful and understandable answers. |
| O3 / NFR1 | Response latency | Cost and latency | Measure end-to-end response time on the test workload. | p90 latency | < 5 s | Students get answers quickly enough to use the tool as a first reference. |
| O3 / NFR4 | Cost per query | Cost and latency | Record model/API usage over the evaluation workload. | Cost per 100 queries | < US$1.00 (illustrative) | The system stays affordable for a student project and possible small-scale use. |

---

## Reflection (Part B)

**Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?**

The most important criterion is **source citation and support** (O1 / FR3 / RAI1 / RAI4). Students cannot easily tell by themselves if an answer about university policy is correct. A wrong answer can lead to a real problem, such as an academic misconduct case. If every answer shows the document, edition and section, and the cited text really supports the answer, the student can check it in the official document before acting. This makes the assistant useful even when it is not perfect.

This criterion links to the Topic 1 quality dimensions of accuracy and transparency, and it reduces the hallucination risk. Answer correctness (≥ 85%) is also important, but a correct answer without a source gives the student no way to confirm it. Latency and cost matter less here, because a slightly slower answer is better than an unsupported one.
