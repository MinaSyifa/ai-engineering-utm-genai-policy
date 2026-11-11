# Week 3 Data Inventory — UTM GenAI Policy Assistant

## Requirement-to-data mapping (Step A1)

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| **FR1 / FR2** (answer from UTM documents) | Text of the UTM GenAI rules and the academic misconduct rules | S01 `utm_genai_guidelines.pdf`; S02 `utm_academic_regulations_16th_ed.pdf` | Official UTM documents. Public, but permission to redistribute is not confirmed, so the PDFs stay local. |
| **FR3 / RAI4** (show source, edition and section) | Document name, edition/date, and page for each piece of text | S01 and S02 plus metadata (file name, URL, SHA-256, page numbers) | The basic whole-document splitter does not keep page numbers. Page-level records are needed to support later citations. |
| **NFR5** (update a document in under 15 minutes) | A repeatable process | The same pipeline and manifest | The pipeline must be rerunnable when UTM publishes a new edition. |

## Data inventory (Step A4)

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| **S01** | `utm_genai_guidelines.pdf` — https://cdex-apps.utm.my/files/guidelines/Guidelines-for-the-Use-of-Generative-Artificial-Intelligence-in-Teaching-and-Learning-UTM.pdf | Unstructured text (PDF). Rules on GenAI use in teaching and learning. Supports FR1, FR3. | Owner: Universiti Teknologi Malaysia. Published 2024 (confirmed by the student from the document). PDF metadata has no title or date (title: None). | Public UTM document. Redistribution not confirmed; file kept local and not committed to GitHub. | 17 pages, 15,189 characters (about 893 per page), no empty pages. Text readable (page 9 sample). Words are sometimes split by extra spaces (e.g. "writi ng", "categori sed"). Mentions: "generative" on 5 pages, "assessment" on 3, "lecturer" on 1 (page 4). PDF page 9 = printed page 8 (offset 1). |
| **S02** | `utm_academic_regulations_16th_ed.pdf` — https://ugs.utm.my/academic-rules | Unstructured text (PDF). Academic Regulations, including Section 7 Academic Misconduct (Articles 37–39). Supports FR2, FR3. | Owner: Universiti Teknologi Malaysia. 16th edition (confirmed by the student). PDF page 5 also shows a publisher line "First Edition 2026" (Penerbit UTM). | Public UTM document. Redistribution not confirmed; file kept local and not committed to GitHub. | 81 pages, 105,112 characters (about 1,298 per page). Almost no text on pages 1, 2, 3, 11, 13, 15, 81 (cover/divider pages). Only PDF page 4 has unreadable `/gid` font codes (1 of 81 pages). Misconduct text readable on PDF pages 59–61 (Article 37, "Academic Deviation"); page 8 is the contents page; page 75 covers examination invigilator duties. PDF page 41 = printed page 24 (offset 17), PDF page 59 = printed page 42. |

## Observed ingestion limitations
- PDF page numbers and printed page numbers differ (S01 offset 1, S02 offset 17). Citations must state which one is used.
- S01 has split words from extraction. This may reduce keyword search quality later.
- S02 page 4 is unreadable; it does not contain policy content that the V1 needs, so it is flagged and not repaired in Week 3.
- PDF metadata does not contain version or date information. The S01 date (2024) and S02 edition (16th) were confirmed by the student from the documents, and S02 also carries a publisher line "First Edition 2026".

## Replacement note
No practice dataset was used. Both sources are the actual project documents.
