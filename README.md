🏥 Hospital Bill Extractor

Automated Layout-Aware Structured Data Extraction for Indian Medical Bills

📌 Overview

This project is an end-to-end document AI pipeline designed to extract structured information from Indian hospital bills (images & scanned PDFs).

The system processes unstructured medical documents and outputs structured JSON containing:

Hospital details

Admission & discharge dates

Bill number & MRN

Billing summary fields

Insurance-related identifiers

The solution combines OCR with a layout-aware transformer model (LayoutLMv3) for structured field extraction.

🧠 Architecture
1️⃣ Preprocessing

PDF to image conversion (if required)

Image normalization

OCR token extraction with bounding boxes

2️⃣ OCR Engine

Tesseract OCR (local)

Language: English

Word-level bounding box extraction

Normalized spatial coordinates (0–1000 scale for LayoutLMv3)

3️⃣ Layout-Aware Field Extraction

Instead of rule-based parsing, the system uses:

LayoutLMv3 (Transformer-based Document Model)

Reformulated as a Question Answering (QA) span prediction task

Each structured field is treated as a question:

"What is the hospital name?"

"What is the bill date?"

"What is the discharge date?"

"What is the MRN number?"

The model predicts the answer span directly from OCR tokens.

This approach improves robustness against layout variations across hospitals.

4️⃣ Output

Standardized JSON schema

Field-level extraction

Span-based predictions

Cleaned and normalized values

Example output:

{
  "hospital_name": "...",
  "bill_number": "...",
  "bill_date": "...",
  "admit_date": "...",
  "discharge_date": "...",
  "mrn": "...",
  "total_amount": "..."
}
📊 Training / Dataset

Dataset Type: Indian hospital bills (non-PII)

Approx. dataset size: 29 documents

76 supervised QA training samples

Document variety:

Multi-page invoices

Itemized billing tables

Insurance sections

Mixed formatting styles

Unlike rule-based systems, this solution uses supervised fine-tuning of LayoutLMv3.

🎯 Key Features

Fully offline (zero API cost)

Layout-aware transformer model

Handles heterogeneous hospital templates

QA-based span prediction (more stable than token classification)

Extracts structured JSON output

Designed for extensibility with more labeled data

📁 Project Structure
├── training_notebook.ipynb
├── inference_demo.ipynb
├── clean_annotations.json
├── model/
│   ├── config.json
│   ├── model.safetensors
│   ├── tokenizer.json
│   └── processor_config.json
└── README.md
⚙️ Installation
pip install -r requirements.txt

Required libraries include:

torch

transformers

pytesseract

pillow

tqdm

scikit-learn

▶️ Usage (Inference Demo)
result = extract_document_fields("sample_bill.jpg")
print(result)

Output:

{
  "hospital_name": "...",
  "bill_date": "...",
  "admit_date": "...",
  "discharge_date": "..."
}
🚀 Performance

Evaluation performed on supervised QA samples:

Exact Match (EM): ~21%

Average F1 Score: ~33%

Performance is expected to improve with:

Larger annotated dataset

Longer fine-tuning

Domain-specific pretraining

Improved OCR consistency

🔒 Notes

Model weights are included in the repository (safetensors format).

All processing is performed locally.

No external paid APIs were used.

👩‍💻 Hackathon Submission

Developed for Artivatic AI Hackathon

The system demonstrates a layout-aware, transformer-based structured extraction pipeline for heterogeneous Indian medical billing documents.
