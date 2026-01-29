---
name: whizz-mind
description: Consult the whizz-mind knowledge base for documentation and answers. Use when the user asks questions that might be answered by stored documentation or when explicitly asked to check whizz-mind.
---

# Whizz Mind Knowledge Base

## Overview

This skill enables access to the **Whizz Mind** knowledge base, a centralized repository of documentation, technical specs, and project knowledge. You should activate this skill whenever the user asks questions that imply the existence of stored documentation, asks about specific project details ("how do I...", "what is the process for..."), or explicitly directs you to "check whizz-mind".

## Workflow

When this skill is active, follow this retrieval-augmented generation (RAG) workflow:

1.  **Search First**: Do not guess. Start by searching the knowledge base.
2.  **Retrieve Details**: content from search results is often truncated. Fetch the full content of relevant documents.
3.  **Synthesize**: Answer the user's question using *only* the information retrieved from Whizz Mind.
4.  **Cite Sources**: Explicitly mention which documents provided the information.

## Tool Usage Guide

### 1. Discovery & Search

*   **`whizz-mind-search-documents`**: The primary entry point.
    *   `query`: The search terms.
    *   `limit`: (Default 10) Adjust based on how broad the topic is.
    *   *Tip:* If the user's query is vague, try multiple specific search queries.

*   **`whizz-mind-list-documents`**: Use this to get a high-level overview of available knowledge or when search yields no results and you need to browse titles.

### 2. Reading Content

*   **`whizz-mind-get-document`**: Retrieves the full document including metadata (author, tags, etc.). Use this when you need context *about* the document.
*   **`whizz-mind-get-only-content-document`**: Retrieves only the text content. Use this for efficient reading when metadata is irrelevant.

### 3. Knowledge Management (User-Directed Only)

Only use these tools if the user explicitly asks to modify the knowledge base:
*   `whizz-mind-add-document`: To create new knowledge entries.
*   `whizz-mind-add-comment`: To annotate existing documents.

## Critical Instructions

*   **Truthfulness**: If the answer is not in Whizz Mind, say "I couldn't find information about [topic] in the Whizz Mind knowledge base." Do not make up answers.
*   **Citation**: Always reference the `title` of the document you used.
*   **Completeness**: If a document seems relevant but incomplete, check for linked documents or related search terms.

