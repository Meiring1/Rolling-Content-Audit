# Rolling Content Audit 

A sanitized reference implementation of a document-driven content governance workflow for enterprise knowledge libraries, using deterministic beacon tagging and controlled repository updates.

## Overview

Large content libraries are difficult to govern at scale. Manual reviews are slow, audit trails are inconsistent, and downstream updates can introduce version drift.

This project demonstrates a two-phase control workflow:

1. **Beacon tagging** — exported content is stamped with deterministic identifiers for traceability
2. **Content update pipeline** — beacon-tagged documents are parsed, transformed into structured HTML, and prepared for repository updates

The public version is intentionally scrubbed and generalized. Sensitive credentials, environment files, vendor-specific identifiers, and proprietary content have been removed.

## Project Components

### `beacon_engine.py`
Adds or updates beacon tags inside exported DOCX content so each response can be tracked deterministically across review cycles.

Core functions:
- detect content entries by ID
- locate the start of each response
- insert a beacon if none exists
- update existing beacon metadata without double-stamping
- preserve original response content in-place

### `content_update_pipeline.py`
Processes a beacon-tagged DOCX and converts each entry into a structured update payload.

Core functions:
- split the document into entry blocks
- extract entry IDs from beacon metadata
- render paragraphs, lists, and tables into HTML
- support dry-run previews before update
- send structured updates through a repository API layer
- handle retry and partial failure scenarios

## Workflow

### Phase 1: Library Tagging
- read exported content from DOCX
- detect each entry by ID
- insert a beacon tag at the start of the response
- save the updated file for verification

### Phase 2: Controlled Content Update
- read the beacon-tagged DOCX
- identify each entry block
- extract the repository entry ID from the beacon
- convert content into structured HTML
- send update payloads to the repository
- report successes and failures

This design allows content changes to remain traceable across document-based review workflows.

## Why This Project Matters

This project is designed to demonstrate:
- Python automation for real business workflows
- deterministic tagging and traceability design
- DOCX parsing and structured content transformation
- safe update patterns with dry-run support
- modular separation between document processing and repository integration
- governance-oriented system design under operational constraints

## Requirements

- Python 3.10+
- `python-docx`
- `requests`

Install dependencies:

```bash
pip install -r requirements.txt# rolling-audit-governance
Reference implementation of an enterprise content governance and rolling audit system.
