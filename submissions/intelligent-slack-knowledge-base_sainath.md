# Intelligent Slack Knowledge Base

---

## Attendee Details

**Name:** Sainath  
**GitHub Username:** Rinzlertron456  
**LinkedIn Profile:** To be added  
**GitHub Project Repository:** https://github.com/Rinzlertron456/intelligent-slack-knowledge-base

---

## Problem Statement Selected

Problem Statement 02: Intelligent Slack Knowledge Base

---

## Project Description

Intelligent Slack Knowledge Base is a permission-aware AI knowledge assistant
that works directly inside Slack. Employees can add PDFs, DOCX files, URLs,
plain text, Slack-hosted files, and past Slack threads, then ask natural-language
questions and receive concise answers grounded only in the indexed company
knowledge.

The system keeps personal, channel/team, and organisation knowledge separate.
It retrieves only content the requesting Slack identity is authorized to access,
includes source citations, supports follow-up questions, summarizes documents,
and explicitly refuses unsupported questions instead of answering from general
model knowledge.

---

## Approach

The solution is RAG-first rather than agent-first. Authorization, retrieval,
evidence thresholds, and citation checks are deterministic; LangGraph coordinates
the query flow, while the LLM only synthesizes from already-authorized evidence.

The ingestion pipeline extracts and normalizes content, creates overlapping
chunks, adds tags, generates OpenAI embeddings, and stores vectors and metadata
in Supabase PostgreSQL with pgvector. Retrieval combines vector similarity with
PostgreSQL full-text ranking and applies workspace, user, and channel filters
inside the database query.

The judging rubric drove the build order: grounded answers and citations first,
Slack-native UX second, access control third, followed by content breadth and
production readiness. A 45-case golden evaluation suite covers answerable,
unanswerable, personal-private, cross-channel, and cross-workspace queries.

---

## Tech Stack and Tools Used

**Frontend:** Slack-native interface; React admin dashboard planned  
**Backend:** Python 3.12, FastAPI, Slack Bolt, LangGraph  
**Database:** Supabase PostgreSQL, pgvector, PostgreSQL full-text search  
**AI Tools/API:** OpenAI Responses API, `gpt-5-mini`, `text-embedding-3-small`  
**Cloud/Deployment:** Docker, local Supabase; hosted deployment prepared  
**Other Tools:** uv, Ruff, Pytest, Git, Slack Socket Mode

---

## Key Features

1. PDF, DOCX, URL, text, Markdown, Slack file, and Slack-thread ingestion.
2. Grounded natural-language answers with deterministic citation validation.
3. Personal, channel/team, and organisation scopes with retrieval-time ACLs.
4. Explicit refusal when evidence is unavailable or inaccessible.
5. Slack slash commands, mentions, direct messages, and threaded follow-ups.
6. Document summaries, auto-tags, recent-document status, and FastAPI health checks.
7. A reproducible 45-case RAG and security evaluation harness.

---

## What is Working?

- OpenAI embedding and answer-model calls are verified.
- Supabase pgvector migration and hybrid retrieval are working.
- End-to-end ingestion, retrieval, generation, citations, and refusal are working.
- Personal and team scope isolation is verified.
- FastAPI `/healthz` and `/readyz` endpoints are working.
- All 21 unit tests pass and Ruff reports no issues.
- Golden evaluation: 100% grounded score, 100% citation validity, 100% refusal
  precision, zero ACL leaks, and 3.782-second p95 latency across 45 synthetic cases.

---

## What is Still in Progress?

- Final Slack app manifest installation and workspace token configuration.
- Hosted Supabase and worker deployment.
- Interactive Slack modals, App Home, and document lifecycle controls.
- React administration and evaluation dashboard.

---

## Screenshots or Demo

**Deployed Link:** Local working prototype  
**Demo Video Link:** To be added  
**Screenshots:** To be added

---

## Challenges Faced

The most important challenge was preventing useful-looking but unauthorized or
unsupported answers. A generic chatbot wrapper would fail the problem statement.
The design therefore enforces access filters in PostgreSQL before generation,
isolates memory by Slack identity and thread, validates citations after
generation, and treats low-confidence retrieval as a refusal.

---

## Learnings

Reliable enterprise RAG depends more on metadata, scope boundaries, retrieval
evaluation, and refusal behavior than on adding many autonomous agents. A compact
deterministic pipeline with measured thresholds was faster, cheaper, and safer.

---

## Future Improvements

- Slack modal workflows, App Home, and lifecycle management.
- OCR and table-aware extraction for scanned and structured documents.
- Contextual chunk headers and reranking for larger corpora.
- Audit views, retention controls, rate limits, and dead-letter handling.
- Authenticated React dashboard for content, scopes, failures, and quality metrics.

---

## Final Note

The project is optimized for the judging criteria rather than raw feature count.
Its differentiators are permission-aware retrieval, evidence-only answers,
measurable refusal behavior, citations, and Slack-native interaction.
