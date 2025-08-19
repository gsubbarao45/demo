
Fine‑Tuning Studio — One‑Page Overview
Purpose
This documentation is prepared for internal employees. If they are not familiar with the procedure, they can directly ask questions and our bot will automatically provide support and guidance. The Fine‑Tuning Studio pipeline enables document ingestion, text extraction, Q/A generation, fine‑tuning, and evaluation to ensure accurate and context‑aware answers.
1) Document Ingestion & Text Extraction
Input: PDF documents (multi‑layout, tables, images, stamps).

Steps:
1. Ingest PDFs into a controlled workspace with lineage (source path, checksum, version).
2. Page Rendering: Convert PDF pages to images (via pdfplumber) to preserve layout cues.
3. Base64 Encoding: Transform images to Base64 for LLM transport.
4. Extraction Prompting: Prompt + Base64 to LLM to extract text, tables, metadata.
5. Post‑processing: Normalize text, remove headers/footers, repair encodings.

Outputs: Clean structured text + JSON tables with lineage.
2) Data Curation & Original Q/A Generation
We create original Q/A exemplars across five types:
- Factual (who/what/when/where)
- Procedural (how‑to)
- Conceptual (definitions)
- Inferential (derive/summarize)
- Reasoning‑based (multi‑hop)

Each item includes: question_type, context_ref, question, answer, citations.
3) Synthetic Data via CoT (Data Curation)
Expand dataset with Chain‑of‑Thought (CoT) prompting:
- Diverse paraphrases & negatives
- Step‑by‑step rationales
- Filters for quality, dedupe, toxicity.

Outputs: Balanced Q/A sets with rationales.
4) Fine‑Tuning
Target Models: LLaMA‑8B‑Instruct, Mosaic‑8B, Mistral‑7B.
Training: SFT with Q/A + CoT, LoRA/QLoRA adapters, curriculum learning.
Artifacts: Model weights, tokenizer, templates, datasheets.
5) Evaluation (LLM‑as‑Judge + Metrics)
LLM‑as‑Judge: rubric‑based scoring for correctness, completeness, faithfulness.
Metrics: EM/F1, ROUGE‑L, BLEU, METEOR, hallucination rate.
Reports: Breakdown by Q/A type, confusion analysis, error exemplars.
Lifecycle
PDF → Images → Base64 → Extraction → Q/A → CoT synthetic → Fine‑tune → Evaluate 
