# Research Copilot — Project Brief

## What it is

A research-to-knowledge system for content creators that structures a topic *before* retrieval, organizes uploaded research against that structure, flags what's missing, and answers questions grounded only in the user's own sources with citations. Unlike standard "chat with your PDF" RAG, the differentiator is a topic-intelligence layer that runs before ingestion, plus an evaluation harness that measures retrieval quality objectively.

## Core components

**1. Research Map Generator**
User enters a topic (e.g. "beginner fitness"). The LLM generates a structured research map: subtopics, real-world search queries, and audience questions. This map becomes the organizing skeleton for everything downstream — it is not just keyword generation, it's a structured research plan the user fills in.

**2. Knowledge Base Builder**
User uploads PDFs, notes, and pasted text. The system chunks and embeds the content into a vector store, and auto-tags each chunk against the subtopics from the research map. Detects duplicate/overlapping content.

**3. Gap Detection**
The system compares the research map (what subtopics *should* be covered) against what's actually present in the uploaded knowledge base (what *is* covered), and tells the user which subtopics are under-researched. This is reasoning over the map-vs-content comparison, not the model recalling world knowledge.

**4. Context-Aware RAG Chat**
User asks questions; the system retrieves relevant chunks and the LLM answers grounded only in uploaded sources. Every answer includes citations (which documents were used) and a source-confidence indicator. Flags conflicting information between sources when present.

**5. Evaluation Harness**
A test harness using the Ragas framework measuring faithfulness, answer relevancy, context precision, and context recall. Includes a documented experiment comparing two chunking strategies (e.g. fixed-size vs semantic) and charting how the metrics change — producing a measurable result for the README.

## Architecture

- One generative LLM (handles research map generation, gap detection, and RAG answering — same model, different prompts)
- One embedding model (text → vectors for similarity search only)
- Vector database for storage and retrieval
- FastAPI backend exposing endpoints for each component
- Local models via Ollama (mistral:7b + nomic-embed-text) to avoid API costs, or swappable for an API model

