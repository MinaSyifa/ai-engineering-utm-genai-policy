# Week 3 Pipeline Decisions — UTM GenAI Policy Assistant

Diagram: `docs/week3_pipeline_diagram.png`
Notebook: `notebooks/week3_pipeline.ipynb`

## 1. Topic 2 requirements this pipeline supports

| Requirement ID | Why the pipeline matters |
|---|---|
| FR1, FR2 | Answers must come from the UTM GenAI Guidelines and the Academic Regulations, so their text must be extracted and prepared. |
| FR3, RAI4 | Every answer must show a source and a page. Each prepared record therefore keeps its source ID, file name and PDF page. |
| RAI1 | Answers must be supported by the source text. Clean, page-aligned records make this check possible later. |
| NFR5 | A new UTM document edition must be added in under 15 minutes. The pipeline is rerunnable and checks its own results. |

## 2. Selected sources

| ID | File | Data type | Pipeline output |
|---|---|---|---|
| S01 | `utm_genai_guidelines.pdf` (17 pages) | Unstructured text, PDF | `data/pipeline_v1/documents_s01/prepared_text.jsonl` |
| S02 | `utm_academic_regulations_16th_ed.pdf` (81 pages) | Unstructured text, PDF | `data/pipeline_v1/documents_s02/prepared_text.jsonl` |

Only the document pathway (Pathway A) is used. The project has no tabular, image, audio, JSON or time-series data.

## 3. Part B comparison and evidence

| Approach / Configuration A | Approach / Configuration B | Observed Evidence | Selected Approach and Trade-off |
|---|---|---|---|
| Fixed-size chunks of 500 characters over the whole document (page numbers lost). | Page-aware chunks of 500 characters: paragraph then line packing, chunks never cross a page, pages with almost no text or unreadable font codes skipped. | **S01:** A = 31 chunks, B = 44. 15 of 31 fixed chunks mix two pages and 15 are cut inside a word. **S02:** A = 208 chunks, B = 249. 70 of 208 fixed chunks mix two pages and 133 are cut inside a word. In S02 the fixed chunk containing "Article 37" started on PDF page 58, but Article 37 is on page 59. The page-aware chunk correctly starts at the Section 7 heading on page 59. | **Selected: B (page-aware).** Benefit: each chunk has a correct page, which supports FR3 and RAI4. Accepted limitation: more chunks (+42% for S01, +20% for S02), a few very short chunks (3 in S01, 10 in S02), and an article that runs over two pages is split in two chunks. |

## 4. Chosen preparation configuration
- Extract text page by page with pypdf.
- Clean repeated spaces and tabs.
- Skip a page if it has fewer than 20 characters of text or contains unreadable `/gid` font codes. This skipped S02 PDF pages 1, 2, 3, 4, 11, 13, 15 and 81 (cover, divider and unreadable pages).
- Chunk size: 500 characters.
- Each record stores: `record_id`, `source_id`, `source` file name, `pdf_page` and `text`.
- `pdf_page` is the PDF page index, **not** the printed page number (S01 offset 1, S02 offset 17). Citations later must state which one they use.

## 5. Accepted limitations
- Chunks follow pages, not articles, so one article can be split across chunks.
- S01 has some words split by extra spaces from PDF extraction (e.g. "writi ng"). They are not repaired in Week 3. This may reduce search quality later and should be reviewed in Topic 4.
- S02 page 4 is unreadable and excluded. It is not a policy page.
- No OCR was needed, because the policy pages are extractable text.

## 6. ETL or ELT decision
**ETL (transform before storing).** The text must be cleaned, split per page and given page and source metadata before it is useful. The data is small (293 records), there is no data warehouse to load raw files into, and the raw PDFs are not stored in the repository. ELT would only save work if large raw data were loaded first and transformed later using a platform, which this project does not need.

## 7. Batch or streaming
**Batch.** UTM publishes new editions rarely, and students do not need updates in real time. The pipeline is run on demand when a document changes (NFR5). Streaming would add complexity without any benefit.

## 8. Output location
All prepared outputs and manifests below are stored in this repository. The source PDFs are kept outside the repository (local copies only).

- Part B output: `data/processed/prepared_text.jsonl` (293 records) and `data/week3_manifest.json`
- Part C output: `data/pipeline_v1/documents_s01/` and `data/pipeline_v1/documents_s02/`, each with `prepared_text.jsonl` and `pipeline_manifest.json`

## 9. Stage contracts

| Stage | Contract / evidence | Failure response | Evidence from this run |
|---|---|---|---|
| **Ingest** | Chosen source exists and is permitted | Stop for missing input | Both PDFs found (1 input each) |
| **Inspect** | Source is readable and has expected properties | Skip unreadable pages and record them; stop if no usable pages remain | S01: 17 of 17 pages used. S02: 73 of 81 pages used, 8 skipped. |
| **Prepare** | Page-aware method produces records with a page number | Stop or report transformation failure | S01: 44 records. S02: 249 records. Matches Part B. |
| **Validate** | Output files exist, are non-empty, with fingerprints | Stop for missing or empty output | Fingerprints and sizes recorded in `pipeline_manifest.json` |
| **Record lineage** | Source links to the exact output | Stop and report missing mapping or metadata | `week3_manifest.json` verified (PASS, 2 records). |

**Repeatability check (Cell 9):** both pipelines were run twice. Source fingerprints, prepared output fingerprints and source-to-output mappings matched (PASS for S01 and S02). This is a smoke test, not full reproducibility across environments.

## 10. Provenance and access caveats
- `source_version` and `access_rights` are recorded as "Not verified" in both manifests.
- The original UTM PDFs are public and are not committed to GitHub; they are available at the official links in the data inventory. The prepared text files (extracted text of those documents) are committed so the pipeline output can be assessed. Formal permission to redistribute the UTM documents is not verified, which is why `access_rights` stays "Not verified" in the manifests.
- Versions: S01 was published in 2024 and S02 is the 16th edition (confirmed by the student from the documents; S02 also shows a publisher line "First Edition 2026"). The `source_version` field in the manifests still says "Not verified" because the pipeline code sets that value automatically; the confirmed versions are recorded in `docs/week3_data_inventory.md`.
- Lineage validation checks that files exist and checksums match. It does not prove data quality or permission to share. Data quality review belongs to Topic 4.

## 11. Traceability

| Objective | Requirement | Pipeline element | Evidence |
|---|---|---|---|
| O1 | FR1, FR2 | Ingest and prepare both UTM documents | 44 + 249 prepared records |
| O1 | FR3, RAI4 | Page-aware chunking with `pdf_page` and source ID in each record | Part B comparison (Article 37 example) |
| O3 | NFR5 | Rerunnable pipeline with validation and lineage | Cell 9 PASS for both sources |
| O2 | RAI1 | Source-to-output lineage with checksums | `week3_manifest.json` PASS |

## 12. Reflection

**Which preparation decision mattered most, and why?**
The most important decision was to keep the page number with every chunk. The fixed-size method looked simpler, but it gave a wrong page for Article 37 (page 58 instead of page 59) and cut 133 of 208 S02 chunks in the middle of a word. If the assistant had used those chunks, it could show students a wrong page in its source citation, which would break FR3 and RAI4. The page-aware method costs more chunks and a few very short ones, but I think a correct citation matters more for a policy assistant than a smaller number of chunks.

**What did I learn or what surprised me?**
I did not expect the quality of the PDFs to differ so much. S01 had split words, S02 had one page with unreadable font codes, and the printed page numbers did not match the PDF page numbers. Checking the real files first (Part A) saved me from building a pipeline on wrong assumptions. I also found that the edition label of S02 on its cover may not match the file name I used, so I now record versions as "not verified" until I check them.

**What I would improve next?**
I would split text by article (for example "Article 37") instead of by page, so that one article stays in one chunk while still keeping its page. I would also check whether the split words in S01 affect search, and decide in Topic 4 whether to repair them. Finally, I should confirm with my lecturer that sharing the prepared text in the repository is acceptable.
