# Week 3 Pipeline Decisions (Topic 3, Part C)

## 1. Topic 2 requirement link
- Requirement ID(s): [FILL IN: e.g. FR__ / RAI__ / NFR__ from docs/week2_requirements.md]
- Architecture link: the Knowledge Source component in docs/week2_architecture_v1.png (approved flood documents, source link, date checked), which feeds Knowledge Access
- Data needed: approved flood document text that can later be retrieved and cited with its source and date

## 2. Selected source
- File: `week3_data_inventory.pdf` (stored in `data/raw/`)
- Data type: PDF document (Pathway A, text)
- Access and rights: [FILL IN: who owns the file and whether it is safe to share]
- Version: not verified (the pipeline records this as "Not verified")

## 3. Part B comparison and evidence

| Approach A: Fixed-size chunks (500 chars) | Approach B: Paragraph-aware chunks (500 chars) | Observed evidence | Selected approach and trade-off |
| --- | --- | --- | --- |
| Cuts text every 500 characters | Keeps paragraphs together, only splits a paragraph if it is longer than 500 characters | Fixed chunks: [FILL IN from Cell 5A1]. Paragraph-aware chunks: [FILL IN from Cell 5A1]. | Paragraph-aware. Benefit: chunks keep complete ideas, so they are easier to read and cite. Accepted limitation: it does not keep page numbers, so `page` is `None`. |

## 4. Chosen preparation configuration
- Cleaning: collapse repeated spaces and tabs, then strip leading and trailing whitespace
- Method: `paragraph-aware`
- Chunk size: 500 characters
- Same configuration is used in Part B (Cell 5A2) and Part C (Cell 8A)

## 5. ETL or ELT decision
ETL. The text is extracted and chunked before it is stored, and only the prepared chunks are saved to `prepared_text.jsonl`. The raw PDF is kept unchanged in `data/raw/`.

## 6. Batch or streaming decision
Batch. The source is one static PDF, so the pipeline is run on demand and does not need to react to live data.

## 7. Output location
- Part B output: `data/processed/prepared_text.jsonl`
- Part B lineage: `data/week3_manifest.json`
- Part C output: `data/pipeline_v1/documents/prepared_text.jsonl`
- Part C run manifest: `data/pipeline_v1/documents/pipeline_manifest.json`

## 8. Stage contracts

| Stage | Contract / evidence | Failure response |
| --- | --- | --- |
| Ingest | `resolve_source()` confirms the PDF exists in `data/raw/` | Stop with FileNotFoundError |
| Inspect | PDF is readable and has extractable text (pages and characters recorded) | Stop with ValueError, note that OCR may be needed |
| Prepare | Paragraph-aware chunking produces at least one chunk | Stop with ValueError |
| Validate | Output file exists, is not empty, SHA-256 and byte size recorded | Stop with an error |
| Record lineage | `pipeline_manifest.json` links the source path and hash to the output | Stop or report missing mapping |

## 9. Repeatability check
Cell 9 reruns the pipeline and compares the source SHA-256, the output SHA-256 and the source-to-output mapping. Result: [FILL IN: paste your PASS message].

## 10. Known caveats
- Page numbers are not kept, so citations cannot point to a page yet.
- The pipeline records the file path and SHA-256, but not the source link or the date checked that the Week 2 Knowledge Source needs. These must be added to the data inventory or a later version of the pipeline.
- Scanned PDFs would fail the inspect stage because there is no OCR step.
- Source version and access rights are marked "Not verified" in the manifest.
- Data quality and Responsible AI checks are left for Topic 4.
