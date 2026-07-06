---
layout: default
title: Upload, Parsing & Chunking
nav_order: 6
---

# Upload, Parsing and Chunking Pipeline

## Overview

This document explains the implementation of the first three stages of the Retrieval-Augmented Generation (RAG) pipeline developed for the **ASK-AI** project. These stages form the preprocessing layer of the system by accepting user documents, extracting their textual content, and preparing them for downstream embedding generation and semantic retrieval.

The pipeline currently consists of:

1. **Document Upload** – Accepts and stores the uploaded file.
2. **Document Parsing** – Extracts plain text from the uploaded document.
3. **Text Chunking** – Splits the extracted text into smaller overlapping chunks.

The next stage of the pipeline is **Embedding Generation**, where each chunk is converted into a vector representation for storage in a vector database.

---

# Pipeline Workflow

```text
                 User Upload
                      │
                      ▼
          ┌────────────────────┐
          │  Document Upload   │
          └────────────────────┘
                      │
                      ▼
          ┌────────────────────┐
          │ Document Parsing   │
          └────────────────────┘
                      │
                      ▼
          ┌────────────────────┐
          │   Text Chunking    │
          └────────────────────┘
                      │
                      ▼
          Embedding Generation
            (Next Pipeline Stage)
```

---

# Stage 1 – Document Upload

## Objective

The upload stage is responsible for receiving a document from the client through the FastAPI endpoint, validating the file type, and saving the document to disk for future processing.

---

## Supported File Types

The system currently accepts the following document formats:

- PDF (`.pdf`)
- Microsoft Word (`.docx`)
- Plain Text (`.txt`)

Files with unsupported extensions are rejected before they are stored.

---

## Upload Process

The upload workflow consists of the following steps:

1. User selects a document in Swagger UI.
2. FastAPI receives the file.
3. The file extension is validated.
4. The uploaded file is read into memory.
5. A unique filename is generated using a UUID.
6. The file is saved inside `storage/uploads/`.
7. The saved file path is returned to the client.

---

## Why UUID?

Different users may upload files having identical filenames.

For example:

```text
report.pdf
```

If another user uploads another `report.pdf`, the first file would normally be overwritten.

Instead, the system saves:

```text
2c6f81bb_report.pdf
```

This guarantees that every uploaded document has a unique filename.

---

## Output

The upload endpoint returns:

```json
{
    "filename": "report.pdf",
    "path": "storage/uploads/2c6f81bb_report.pdf",
    "uploaded_at": "2026-07-06T12:30:41"
}
```

---

# Stage 2 – Document Parsing

## Objective

After a document has been uploaded successfully, its textual content must be extracted.

Since different document formats store text differently, each file type requires its own parser.

The parser converts every supported document into a single plain-text string.

---

## PDF Parsing

PDF files are parsed using the **pypdf** library.

Each page is processed individually.

```text
PDF

Page 1
Page 2
Page 3

↓

Combined Text
```

The extracted text from every page is appended together to create one complete document.

---

## DOCX Parsing

Microsoft Word documents are parsed using **python-docx**.

Each paragraph is read separately before combining them into one text string.

```text
Paragraph 1

Paragraph 2

Paragraph 3

↓

Combined Text
```

---

## TXT Parsing

Text files already contain plain text.

The parser simply opens the file using UTF-8 encoding and reads the entire content.

---

## Dispatcher Function

Instead of checking file types throughout the project, a single function named:

```python
load_document()
```

automatically determines which parser to use based on the file extension.

For example:

```text
.pdf   → load_pdf()

.docx  → load_docx()

.txt   → load_txt()
```

This makes the remaining pipeline independent of the document format.

---

## Parsing Output

Regardless of the input format, the parser always returns a plain text string.

Example:

```text
Artificial Intelligence (AI) is a branch of computer science
that focuses on creating intelligent systems capable of learning,
reasoning and decision making...
```

This standardized output allows all later stages of the pipeline to work identically for every document type.

---

# Stage 3 – Text Chunking

## Objective

Large Language Models and embedding models cannot efficiently process very large documents at once because they have limited context windows.

Instead of embedding an entire document, the extracted text is divided into smaller sections called **chunks**.

Each chunk will later become an individual vector embedding.

---

## Chunking Strategy

The current implementation uses **character-based chunking**.

Default configuration:

| Parameter | Value |
|-----------|------:|
| Chunk Size | 500 characters |
| Overlap | 50 characters |

---

## Example

```text
Chunk 1

Artificial Intelligence is a branch of computer science...
.........................................................

--------------------------------------------

Chunk 2

.................computer science............
.............................................
```

Notice that the beginning of **Chunk 2** contains some text from the end of **Chunk 1**.

This is called **overlap**.

---

## Why Overlap?

Without overlap:

```text
Chunk 1

Artificial Intelligence is a branch

Chunk 2

of computer science...
```

The sentence becomes split across two chunks.

Neither chunk contains the complete idea.

With overlap:

```text
Chunk 1

Artificial Intelligence is a branch

Chunk 2

is a branch of computer science...
```

Both chunks preserve the surrounding context.

This significantly improves semantic retrieval.

---

## Benefits of Chunking

- Faster embedding generation
- Better semantic search accuracy
- Improved retrieval quality
- Smaller context for the LLM
- Reduced memory usage
- Preserves context through overlapping text

---

# Project Structure

```text
fastapi_service/

│
├── main.py
│
├── routers/
│   └── upload.py
│
├── schemas/
│   └── document.py
│
├── services/
│   ├── storage.py
│   ├── document_loader.py
│   └── chunking.py
│
├── storage/
│   └── uploads/
│
└── requirements.txt
```

---

# Testing

The pipeline was tested locally using FastAPI's built-in Swagger UI.

---

## Install Dependencies

```bash
pip install fastapi uvicorn python-multipart pypdf python-docx
```

---

## Run the Server

```bash
uvicorn main:app --reload
```

If successful, FastAPI displays:

```text
INFO: Uvicorn running on http://127.0.0.1:8000
```

---

## Open Swagger UI

Open your browser and navigate to:

```text
http://127.0.0.1:8000/docs
```

Swagger UI is automatically generated by FastAPI.

---

## Test Procedure

1. Expand **POST /documents/upload**
2. Click **Try it out**
3. Select a PDF, DOCX or TXT file.
4. Click **Execute**

If successful, Swagger returns:

```json
{
  "filename": "sample.pdf",
  "path": "storage/uploads/7fa32c_sample.pdf",
  "uploaded_at": "2026-07-06T12:10:32"
}
```

---

## Verifying Parsing

After uploading a document, the parsed text was printed to the terminal using:

```python
print(text[:500])
```

A readable output confirms that the parser extracted the document successfully.

---

## Verifying Chunking

After chunking, the following values were printed:

```python
print(len(chunks))
print(chunks[0])
```

Successful chunking is confirmed when:

- Parsed text length is greater than zero.
- More than one chunk is generated for sufficiently large documents.
- Consecutive chunks share the configured overlap.

---

# Conclusion

The Upload, Parsing, and Chunking pipeline establishes the foundation of the ASK-AI Retrieval-Augmented Generation (RAG) system. User documents are securely uploaded, validated, parsed into plain text, and segmented into overlapping chunks that preserve contextual continuity. These chunks are optimized for the next stages of the pipeline, where they will be converted into vector embeddings and indexed in a vector database for efficient semantic retrieval and LLM-powered question answering.