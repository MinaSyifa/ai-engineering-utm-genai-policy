# UTM GenAI Policy Assistant

> A document-grounded assistant that answers UTM undergraduate students' questions about acceptable use of generative AI, with citations to official university documents.

## Problem Statement
Students are often unsure what they may do with generative AI in coursework. The rules are spread across several UTM documents and can change by course and assessment. AI engineering allows an assistant that answers from the official documents only, shows its sources, and refers students to their lecturer when the rule depends on the assessment.

## Evidence of the Problem
- UTM guidelines require GenAI use to follow the Academic Regulations and require lecturers to state GenAI rules for each assessment, so students must check several sources (UTM, *Guidelines for the Use of Generative AI in Teaching and Learning*).
- Published work reports that students struggle to navigate complex academic policies (Torkian & Zhou, *Carolina Guide*, arXiv:2606.28360).

## Initial Related Work / Existing Approaches
- Carolina Guide (Torkian & Zhou, arXiv:2606.28360) — RAG with guardrails for academic policy. Limitation: single university, does not cover AI-use rules.
- Bournemouth RAG chatbot (Meacham et al., *AI & Society*) — high faithfulness on a research degree code of practice. Limitation: one document, simulated user feedback.
- RMUTTOBot (TAG-based RAG chatbot) — admissions services. Limitation: admissions focus, generic answers to broad policy questions.

## Engineering Gap / Limitation
No existing assistant combines several UTM documents to answer "Can I use AI for this?" with citations and clear uncertainty when the rule depends on the course or assessment.

## Project Objectives
- O1: To design a document-grounded assistant for UTM generative AI policy questions.
- O2: To develop it with retrieval, citations, and guardrails against giving final misconduct rulings.
- O3: To evaluate correctness, groundedness, latency, and safe refusal.

## Target Users
UTM undergraduate students.

## Data Sources
- UTM Guidelines for the Use of Generative AI in Teaching and Learning — https://cdex-apps.utm.my/files/guidelines/Guidelines-for-the-Use-of-Generative-Artificial-Intelligence-in-Teaching-and-Learning-UTM.pdf
- UTM Academic Regulations, Full-Time Bachelor's Degree Programme (16th edition) — https://ugs.utm.my/academic-rules

## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |
