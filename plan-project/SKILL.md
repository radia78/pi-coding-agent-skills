---
name: plan-project
description: Generate a concise plan for a coding project, covering the goal, repo structure, language and frameworks, and architecture diagram.
---

# Plan Project

Plan a coding project by laying out its goal, repo structure, tech stack, and architecture. The point is to settle the big decisions (what files exist, what tools are used, how data flows) before any code is written, while staying small enough that the user can read it in under a minute.

## Steps

1. Reduce the user's request to one overall goal. Take the language, frameworks, and constraints they already stated; don't re-ask them.
2. If a missing detail would change the whole design (web app vs. CLI, language, where it runs), ask one short question. Otherwise pick the simplest option and mark it as an assumption.
3. Propose the repo structure, choose the stack, and sketch the architecture.
4. Present the plan in the format below. Build the project only if the user asks; otherwise offer to start.

## Plan format

Use exactly these four sections, in this order:

**Goal**
One or two sentences: the project's purpose and what "working" looks like.

**Repo Structure**
A file tree in a code block. Annotate each file or folder with a few words on its role. Include only files the project truly needs.

**Language & Frameworks**
The language (with version if it matters) and each library or framework, with a few words on why it's needed. Mark each as *specified* (the user asked for it) or *assumed* (your choice).

**Architecture**
A simple diagram in a code block showing how the modules connect and how data or control flows between them (ASCII arrows by default; Mermaid only if the user asks). Label the arrows when the thing being passed isn't obvious.

## Guidelines

- Be brief. Use the fewest words that stay clear; bullets and fragments beat sentences.
- Fewer dependencies is better. Prefer the standard library, and add a framework only when it clearly saves significant work.
- Keep the repo flat and obvious. Don't add `utils/`, config layers, or abstractions the project doesn't yet need; a handful of files is often enough.
- Keep the architecture to the main path: usually 3 to 6 boxes. Leave out error handling, logging, and deployment unless the user asks.
- Match the scale to the request. A weekend project shouldn't get microservices, a database, or Docker unless it's needed.
- Flag a serious risk (for example credentials, destructive operations, or paid APIs) in one line at the end; add nothing else beyond the four sections.

## Example

**User:** I want a tool that watches a folder of receipts, extracts totals, and gives me a monthly spending summary.

**Goal**
CLI tool that reads receipt files from a folder and prints a monthly spending summary.

**Repo Structure**
```
receipt-tracker/
├── README.md
├── requirements.txt
├── main.py          # CLI entry point
├── extract.py       # pull date and total from a receipt
├── summarize.py     # group totals by month
└── receipts/        # input files
```

**Language & Frameworks**
- Python 3.11 (assumed)
- `pytesseract` + `Pillow`: OCR for image receipts (assumed)
- `argparse`, `csv`, `pathlib` (standard library): CLI, output, file handling

**Architecture**
```
receipts/ --files--> extract.py --(date, total)--> summarize.py --> main.py --> printed summary
```

Note: OCR can misread totals, so print the extracted values for the user to check.