# Week 1 Project Scope

**Project Name**
UTM GenAI Policy Assistant

**Domain**
Higher Education / University Policy and Academic Integrity

**Problem Statement**
Students are often unsure what they are allowed to do with generative AI in their coursework and assessments. The rules are spread across several university documents, and each lecturer may set different rules for each assessment. Because of this, students may break the rules by mistake, and staff spend time answering the same questions again and again. AI engineering makes it possible to build an assistant that answers questions using only the official documents and shows the source of every answer.

**Evidence of the Problem**
Evidence 1: UTM's guidelines say that the use of generative AI must follow the Academic Regulations and other related university documents, and that lecturers must tell students how GenAI may be used in each assessment. This means a student must read more than one document and check each course, which is difficult to do.
Source / citation: Universiti Teknologi Malaysia. *Guidelines for the Use of Generative Artificial Intelligence in Teaching and Learning.* https://cdex-apps.utm.my/files/guidelines/Guidelines-for-the-Use-of-Generative-Artificial-Intelligence-in-Teaching-and-Learning-UTM.pdf

Evidence 2: Published work on university policy assistants states that students often struggle to find their way through complex academic policies, which causes delays in getting correct information.
Source / citation: Torkian, B. & Zhou, J. *Carolina Guide: A Multi-Agent RAG System with Institutional Guardrails for Academic Policy Assistance.* arXiv:2606.28360. https://arxiv.org/abs/2606.28360

**Initial Related Work (at least 3)**
Study / solution 1: Carolina Guide, a multi-agent RAG assistant for academic policy at the University of South Carolina.
Source / citation: Ben Torkian & Jun Zhou, arXiv:2606.28360.
Key limitation: It is built for one university's policies and refuses personalised advising, so it does not cover AI-use rules or the idea that rules differ by assessment.

Study / solution 2: RAG chatbot for Bournemouth University's Code of Practice for Research Degrees, evaluated with RAGAS (faithfulness 0.9597) and simulated student feedback.
Source / citation: Meacham, S. et al. *Towards Trustworthy AI for Academic Policy Navigation: A Human Evaluation of a RAG-Powered Chatbot.* AI & Society (Springer Nature). https://staffprofiles.bournemouth.ac.uk/display/journal-article/405455
Key limitation: It covers one document for research degree students, and the user feedback was simulated rather than from real users.

Study / solution 3: RMUTTOBot, a table-augmented RAG chatbot for university admission services.
Source / citation: *RMUTTOBot: Transforming University Admission Services with a TAG-based RAG LLM Chatbot.* https://li01.tci-thaijo.org/index.php/rmutsvrj/article/view/267581
Key limitation: It focuses on admissions, and the authors note that standard RAG can return generic answers to broad policy questions.

**Engineering Gap / Limitation**
Existing university assistants focus on admissions, advising or single policy documents. Gap: there is no assistant for the question "Can I use AI for this?" that combines several UTM documents, gives answers with citations, and clearly tells the student to check their lecturer when the rule depends on the course or assessment.

**Initial Project Objectives (2–3)**
O1: To design a document-grounded assistant that answers questions about generative AI use using official UTM documents.
O2: To develop the assistant using retrieval, source citations, and guardrails that refuse to give final rulings on misconduct cases.
O3: To evaluate the assistant for answer correctness, groundedness (answers supported by the documents), latency, and safe refusal behaviour.

**Target Users**
UTM undergraduate students who need quick and clear guidance on acceptable use of generative AI in their coursework.

**Data Sources (at least 2)**
Source 1: UTM Guidelines for the Use of Generative AI in Teaching and Learning — URL: https://cdex-apps.utm.my/files/guidelines/Guidelines-for-the-Use-of-Generative-Artificial-Intelligence-in-Teaching-and-Learning-UTM.pdf
Source 2: UTM Academic Regulations, Full-Time Bachelor's Degree Programme (16th edition, includes the Academic Misconduct section) — URL: https://ugs.utm.my/academic-rules
Source 3 (if any): not confirmed yet — to be added if another public UTM document on exams or misconduct is found

**Similar to EduAdvisor**
Both are document-grounded Q&A assistants for university students, using retrieval, citations and guardrails, with public university documents as data.

**Different from EduAdvisor**
EduAdvisor answers course and graduation questions. This project answers AI-use and academic integrity questions, where the correct answer can depend on the lecturer's rule for each assessment. So the assistant must show uncertainty and refer students to their lecturer instead of guessing. It also needs a stronger refusal guardrail because a wrong answer could lead to an integrity problem for the student.

**Responsible AI Risks (at least 2)**
Risk 1 — Hallucination: The model states a rule that does not exist in the UTM documents → the student uses AI in a way that is not allowed and may face an academic misconduct case.
Risk 2 — Over-confidence: The assistant says "yes, you can use AI" when the lecturer's assessment rules say no → the student is misled and is still responsible for the breach.
Risk 3 — Fairness / language: Questions in Malay or informal English get worse answers than formal English → unequal access to guidance.
Risk 4 — Outdated documents: Policies are updated but the assistant still uses an old version → wrong guidance.
