# Local Software Intelligence Agent - Scoped Roadmap

## Vision

Build a fully local software intelligence agent capable of understanding:

* Source code
* Documentation
* Configurations
* Architecture
* Git history
* Operational patterns
* ML pipelines

The system should answer natural language developer questions using:

* Semantic retrieval
* Symbolic search
* AST analysis
* Git intelligence
* Lightweight reasoning

The system is intentionally designed for:

* Local/offline usage
* Privacy-sensitive repositories
* Low-resource machines
* Micro-LLM / SLM inference
* Deterministic retrieval-first architecture

---

# 1. Core Product Principles

## Local Only

The system must:

* Never send code externally
* Work completely offline
* Use local embeddings
* Use local LLM inference only
* Support air-gapped environments

---

## Retrieval First, LLM Second

The architecture should prioritize:

1. Deterministic retrieval
2. Symbolic indexing
3. AST understanding
4. Graph traversal
5. Git metadata

LLM reasoning should only synthesize results.

The LLM should NOT:

* Crawl repositories directly
* Brute-force search files
* Hallucinate architecture

---

## Small Model Requirement (Important)

The system MUST work with micro LLMs / SLMs.

### Examples

| Model               | Class       |
| ------------------- | ----------- |
| Qwen2.5 1.5B        | Micro       |
| Qwen2.5 3B          | SLM         |
| Phi-3 Mini          | SLM         |
| Gemma 2B            | Micro       |
| TinyLlama           | Tiny        |
| DeepSeek Coder 1.3B | Micro       |
| SmolLM              | Ultra-small |

---

## Why Small Models?

Because most intelligence comes from:

* Indexing
* Retrieval
* Metadata
* AST analysis
* Git context

Not from giant reasoning models.

The LLM only needs to:

* Summarize
* Rank
* Explain
* Synthesize

This dramatically reduces:

* Memory
* Latency
* Hardware requirements
* Hallucinations

---

# 2. High-Level Architecture

```text
                ┌─────────────────────┐
                │ CLI / Claude Agent  │
                └──────────┬──────────┘
                           │
                ┌──────────▼──────────┐
                │ Query Intent Router │
                └──────────┬──────────┘
                           │
     ┌─────────────────────┼─────────────────────┐
     │                     │                     │
┌────▼────┐        ┌──────▼──────┐      ┌──────▼──────┐
│ Vector  │        │ Symbol/FTS  │      │ Git Adapter │
│ Search  │        │ AST Search  │      │ Metadata    │
└────┬────┘        └──────┬──────┘      └──────┬──────┘
     │                    │                    │
     └────────────────────┼────────────────────┘
                          │
                ┌─────────▼─────────┐
                │ Context Builder   │
                └─────────┬─────────┘
                          │
                ┌─────────▼─────────┐
                │ Micro LLM / SLM   │
                └───────────────────┘
```

---

# 3. Supported Question Categories

The roadmap is organized around the types of questions the agent must answer.

---

# PHASE 1 - Foundational Repository Search

## Goal

Enable intelligent local retrieval over:

* Code
* Markdown
* Configs
* Documents

This phase answers:

> "Where is X?"

without deep reasoning.

---

## Capabilities

### Filesystem Scanning

* Recursive scanning
* Incremental indexing
* File metadata storage
* Chunk extraction

---

### Ignore / Allow Rules

Support:

```yaml
ignore:
  - node_modules/**
  - .git/**
  - dist/**
```

and:

```yaml
mode: allowlist

allow:
  - apps/**
  - docs/**
```

---

## Hybrid Search

### Semantic Search

Questions like:

* Find yesterday’s standup report
* Where are deployment docs?
* Find docs mentioning OAuth

---

### Exact / FTS Search

Questions like:

* Where is OpenAI API key stored?
* Which files mention Kafka?
* Where is JWT configured?

---

## Questions Answered in Phase 1

### Document Discovery

* Find yesterday’s standup report
* Find onboarding docs
* Where are deployment steps documented?
* Which markdown files mention OAuth?

---

### Config Discovery

* Where is Redis configured?
* Which services use PostgreSQL?
* Where are timeouts configured?
* Which configs contain API keys?

---

### Secret Discovery

* Where is OpenAI secret key stored?
* Are any AWS keys hardcoded?
* Which files contain private certificates?

---

### API / Text Search

* Which services mention Kafka topic X?
* Where is endpoint `/refund` defined?
* Which files reference environment variable X?

---

## Required Technologies

| Concern    | Recommendation         |
| ---------- | ---------------------- |
| Vector DB  | LanceDB                |
| FTS        | SQLite FTS5            |
| Embeddings | bge-small-en           |
| Chunking   | Function/section based |
| Watchers   | watchdog               |

---

# PHASE 2 - Code Structure Intelligence

## Goal

Understand code structure and symbols.

This phase answers:

> "What code implements X?"

---

## Capabilities

### AST Parsing

Use:

* tree-sitter
* Language-aware parsing

---

### Symbol Extraction

Track:

* Classes
* Functions
* Interfaces
* Imports
* Constants
* Configs

---

### Relationship Mapping

Track:

* Imports
* Inheritance
* Usage references
* Call references

---

## Questions Answered in Phase 2

### Implementation Discovery

* How is PAN detection implemented?
* Where is retry logic implemented?
* Which services publish Kafka events?
* How does authentication work?

---

### Utility Discovery

* Any common code for `any → int` conversion?
* Where is caching implemented?
* Which files implement validation?

---

### Dependency Analysis

* Which services depend on module X?
* What uses this DTO?
* Which files import utility Y?
* Is this function unused?

---

### API Discovery

* Which APIs require authentication?
* Where are GraphQL resolvers?
* Which services expose gRPC endpoints?

---

### Architecture Questions

* What is entry point for service X?
* Which services are involved in checkout flow?
* How does request tracing propagate?

---

## Required Technologies

| Concern   | Recommendation              |
| --------- | --------------------------- |
| Parsing   | tree-sitter                 |
| Symbol DB | SQLite                      |
| Graph     | Lightweight adjacency store |
| Search    | Hybrid semantic + symbolic  |

---

# PHASE 3 - Git & Historical Intelligence

## Goal

Understand:

* Changes
* Ownership
* Evolution
* Releases

This phase answers:

> "Who changed X and when?"

---

## Capabilities

### Git Metadata

Support:

* `git log`
* `git blame`
* Branch discovery
* Commit summaries

---

### Change Tracking

Track:

* Modified files
* Ownership
* Frequency
* Release relationships

---

## Questions Answered in Phase 3

### Release Intelligence

* What is last release branch of application X?
* Which release introduced API v2?
* What changed between release 2.1 and 2.3?

---

### Ownership

* Who modified this config most often?
* Who introduced this bug?
* Who owns payment module?

---

### Historical Queries

* When was config X changed?
* Which commits touched fraud detection?
* When was retry logic added?

---

### Repository Evolution

* Which files change most frequently?
* Which modules are unstable?
* What areas changed heavily this month?

---

## Required Technologies

| Concern        | Recommendation |
| -------------- | -------------- |
| Git library    | pygit2         |
| Metadata cache | SQLite         |
| Diff parser    | Git native     |

---

# PHASE 4 - Architectural & Semantic Reasoning

## Goal

Provide system-level understanding.

This phase answers:

> "Explain how the system works."

---

## Capabilities

### Semantic Flow Reconstruction

Build:

* Execution traces
* Dependency graphs
* Service relationships

---

### Multi-hop Retrieval

Follow:

* Imports
* References
* Service calls
* Event chains

---

### Context Synthesis

Use micro-LLM for:

* Explanations
* Summaries
* Architecture understanding

---

## Questions Answered in Phase 4

### System Understanding

* How does checkout work end-to-end?
* Which services participate in onboarding?
* How is request tracing implemented?
* What triggers invoice generation?

---

### Operational Intelligence

* Where are retries implemented?
* Which services have health checks?
* Where is rate limiting implemented?
* Which code suppresses exceptions?

---

### ML / AI Understanding

* Which models use XGBoost?
* What features are used in fraud model?
* Where are prompts stored?
* How is RAG implemented?

---

### Runtime & Observability

* Where are logs emitted?
* Which services use OpenTelemetry?
* Where are metrics defined?

---

## Required Technologies

| Concern         | Recommendation             |
| --------------- | -------------------------- |
| Graph traversal | NetworkX/lightweight graph |
| Re-ranking      | Hybrid scoring             |
| Summarization   | Small local LLM            |

---

# PHASE 5 - Pattern & Quality Intelligence

## Goal

Understand software quality and patterns.

This phase answers:

> "How good or maintainable is this system?"

---

## Capabilities

### Pattern Detection

Detect:

* Strategy pattern
* Factory pattern
* Repository pattern
* CQRS
* Middleware chains

---

### Smell Detection

Identify:

* Duplicate logic
* Dead code
* Oversized classes
* Cyclic dependencies
* Risky modules

---

## Questions Answered in Phase 5

### Pattern Discovery

* Any references to Strategy pattern?
* Where are decorators used?
* Which modules use CQRS?

---

### Quality Analysis

* Which modules violate SRP?
* Where is duplicate retry logic?
* Which functions are overly complex?
* Which code is tightly coupled?

---

### Refactoring Assistance

* What breaks if module X is removed?
* Which utilities are duplicated?
* Which APIs are deprecated?

---

## Required Technologies

| Concern        | Recommendation       |
| -------------- | -------------------- |
| AST heuristics | Custom               |
| Graph analysis | Dependency graph     |
| Similarity     | Embedding clustering |

---

# PHASE 6 - Advanced Software Intelligence (Future)

## Goal

Move toward intelligent repository reasoning.

---

## Questions Answered

### High-Level Reasoning

* Explain checkout architecture to new developer
* Compare authentication across services
* Suggest best place to add caching
* Identify risky upgrade areas
* Find inconsistent validation logic

---

### Cross-Repository Intelligence

* Which repos use shared auth SDK?
* Which services still use API v1?
* Which repos depend on common package X?

---

### Predictive Intelligence

* Which modules are most fragile?
* Which files likely cause outages?
* Which code paths lack observability?

---

# 4. Recommended Local Models

## Embeddings

| Model            | Notes                     |
| ---------------- | ------------------------- |
| bge-small-en     | Best balance              |
| nomic-embed-text | Strong semantic retrieval |
| e5-small         | Lightweight               |

---

## Generation Models

| Model               | Use              |
| ------------------- | ---------------- |
| Qwen2.5 1.5B        | Default          |
| Phi-3 Mini          | Strong reasoning |
| Gemma 2B            | Lightweight      |
| TinyLlama           | Ultra-fast       |
| DeepSeek Coder 1.3B | Code-focused     |

---

# 5. Critical Design Principle

The system succeeds because:

* Retrieval is strong
* Indexing is rich
* Metadata is structured

Not because the LLM is large.

This architecture intentionally minimizes dependence on:

* Giant context windows
* Expensive inference
* Cloud reasoning

The ideal flow is:

```text
Repository Intelligence
        ↓
Structured Retrieval
        ↓
Focused Context
        ↓
Small Local Model
        ↓
Concise Answer
```

---

# 6. Recommended MVP

## Absolute MVP

### Must Have

* Filesystem indexing
* Ignore/allow rules
* Semantic search
* Exact search
* Line references
* Local embeddings
* Incremental indexing

---

## First Valuable Questions

* Where is OpenAI key stored?
* Find yesterday standup report
* Where is retry logic implemented?
* What config uses Redis?
* Which files mention Kafka?

---

# 7. Long-Term Positioning

This is NOT:

* A chatbot over code
* A simple vector search app

It is:

> A Local Software Intelligence System

focused on:

* Repository understanding
* Architectural reasoning
* Developer productivity
* Operational awareness
* Private/offline environments

using:

* Micro-LLMs
* Deterministic retrieval
* Structured indexing
* Lightweight reasoning
