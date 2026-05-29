Local Software Intelligence Agent — Scoped Roadmap

Vision

Build a fully local software intelligence agent capable of understanding:

* source code
* documentation
* configurations
* architecture
* git history
* operational patterns
* ML pipelines

The system should answer natural language developer questions using:

* semantic retrieval
* symbolic search
* AST analysis
* git intelligence
* lightweight reasoning

The system is intentionally designed for:

* local/offline usage
* privacy-sensitive repositories
* low-resource machines
* micro-LLM / SLM inference
* deterministic retrieval-first architecture

⸻

1. Core Product Principles

Local Only

The system must:

* never send code externally
* work completely offline
* use local embeddings
* use local LLM inference only
* support air-gapped environments

⸻

Retrieval First, LLM Second

The architecture should prioritize:

1. deterministic retrieval
2. symbolic indexing
3. AST understanding
4. graph traversal
5. git metadata

LLM reasoning should only synthesize results.

The LLM should NOT:

* crawl repositories directly
* brute-force search files
* hallucinate architecture

⸻

Small Model Requirement (Important)

The system MUST work with:

Micro LLMs / SLMs

Examples:

Model	Class
Qwen2.5 1.5B	Micro
Qwen2.5 3B	SLM
Phi-3 Mini	SLM
Gemma 2B	Micro
TinyLlama	Tiny
DeepSeek Coder 1.3B	Micro
SmolLM	Ultra-small

⸻

Why Small Models?

Because most intelligence comes from:

* indexing
* retrieval
* metadata
* AST analysis
* git context

NOT from giant reasoning models.

The LLM only needs to:

* summarize
* rank
* explain
* synthesize

This dramatically reduces:

* memory
* latency
* hardware requirements
* hallucinations

⸻

2. High-Level Architecture

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

⸻

3. Supported Question Categories

The roadmap is organized around the types of questions the agent must answer.

⸻

PHASE 1 — Foundational Repository Search

Goal

Enable intelligent local retrieval over:

* code
* markdown
* configs
* documents

This phase answers:

“Where is X?”

without deep reasoning.

⸻

Capabilities

Filesystem Scanning

* recursive scanning
* incremental indexing
* file metadata storage
* chunk extraction

⸻

Ignore / Allow Rules

Support:

ignore:
  - node_modules/**
  - .git/**
  - dist/**

and:

mode: allowlist
allow:
  - apps/**
  - docs/**

⸻

Hybrid Search

Semantic Search

Questions like:

* Find yesterday’s standup report
* Where are deployment docs?
* Find docs mentioning OAuth

⸻

Exact / FTS Search

Questions like:

* Where is OpenAI API key stored?
* Which files mention Kafka?
* Where is JWT configured?

⸻

Questions Answered in Phase 1

Document Discovery

* Find yesterday’s standup report
* Find onboarding docs
* Where are deployment steps documented?
* Which markdown files mention OAuth?

⸻

Config Discovery

* Where is Redis configured?
* Which services use PostgreSQL?
* Where are timeouts configured?
* Which configs contain API keys?

⸻

Secret Discovery

* Where is OpenAI secret key stored?
* Are any AWS keys hardcoded?
* Which files contain private certificates?

⸻

API / Text Search

* Which services mention Kafka topic X?
* Where is endpoint /refund defined?
* Which files reference environment variable X?

⸻

Required Technologies

Concern	Recommendation
Vector DB	LanceDB
FTS	SQLite FTS5
Embeddings	bge-small-en
Chunking	function/section based
Watchers	watchdog

⸻

PHASE 2 — Code Structure Intelligence

Goal

Understand code structure and symbols.

This phase answers:

“What code implements X?”

⸻

Capabilities

AST Parsing

Use:

* tree-sitter
* language-aware parsing

⸻

Symbol Extraction

Track:

* classes
* functions
* interfaces
* imports
* constants
* configs

⸻

Relationship Mapping

Track:

* imports
* inheritance
* usage references
* call references

⸻

Questions Answered in Phase 2

Implementation Discovery

* How is PAN detection implemented?
* Where is retry logic implemented?
* Which services publish Kafka events?
* How does authentication work?

⸻

Utility Discovery

* Any common code for any → int conversion?
* Where is caching implemented?
* Which files implement validation?

⸻

Dependency Analysis

* Which services depend on module X?
* What uses this DTO?
* Which files import utility Y?
* Is this function unused?

⸻

API Discovery

* Which APIs require authentication?
* Where are GraphQL resolvers?
* Which services expose gRPC endpoints?

⸻

Architecture Questions

* What is entry point for service X?
* Which services are involved in checkout flow?
* How does request tracing propagate?

⸻

Required Technologies

Concern	Recommendation
Parsing	tree-sitter
Symbol DB	SQLite
Graph	lightweight adjacency store
Search	hybrid semantic + symbolic

⸻

PHASE 3 — Git & Historical Intelligence

Goal

Understand:

* changes
* ownership
* evolution
* releases

This phase answers:

“Who changed X and when?”

⸻

Capabilities

Git Metadata

Support:

* git log
* git blame
* branch discovery
* commit summaries

⸻

Change Tracking

Track:

* modified files
* ownership
* frequency
* release relationships

⸻

Questions Answered in Phase 3

Release Intelligence

* What is last release branch of application X?
* Which release introduced API v2?
* What changed between release 2.1 and 2.3?

⸻

Ownership

* Who modified this config most often?
* Who introduced this bug?
* Who owns payment module?

⸻

Historical Queries

* When was config X changed?
* Which commits touched fraud detection?
* When was retry logic added?

⸻

Repository Evolution

* Which files change most frequently?
* Which modules are unstable?
* What areas changed heavily this month?

⸻

Required Technologies

Concern	Recommendation
Git library	pygit2
Metadata cache	SQLite
Diff parser	git native

⸻

PHASE 4 — Architectural & Semantic Reasoning

Goal

Provide system-level understanding.

This phase answers:

“Explain how the system works.”

⸻

Capabilities

Semantic Flow Reconstruction

Build:

* execution traces
* dependency graphs
* service relationships

⸻

Multi-hop Retrieval

Follow:

* imports
* references
* service calls
* event chains

⸻

Context Synthesis

Use micro-LLM for:

* explanations
* summaries
* architecture understanding

⸻

Questions Answered in Phase 4

System Understanding

* How does checkout work end-to-end?
* Which services participate in onboarding?
* How is request tracing implemented?
* What triggers invoice generation?

⸻

Operational Intelligence

* Where are retries implemented?
* Which services have health checks?
* Where is rate limiting implemented?
* Which code suppresses exceptions?

⸻

ML / AI Understanding

* Which models use XGBoost?
* What features are used in fraud model?
* Where are prompts stored?
* How is RAG implemented?

⸻

Runtime & Observability

* Where are logs emitted?
* Which services use OpenTelemetry?
* Where are metrics defined?

⸻

Required Technologies

Concern	Recommendation
Graph traversal	NetworkX/lightweight graph
Re-ranking	hybrid scoring
Summarization	small local LLM

⸻

PHASE 5 — Pattern & Quality Intelligence

Goal

Understand software quality and patterns.

This phase answers:

“How good or maintainable is this system?”

⸻

Capabilities

Pattern Detection

Detect:

* Strategy pattern
* Factory pattern
* Repository pattern
* CQRS
* middleware chains

⸻

Smell Detection

Identify:

* duplicate logic
* dead code
* oversized classes
* cyclic dependencies
* risky modules

⸻

Questions Answered in Phase 5

Pattern Discovery

* Any references to Strategy pattern?
* Where are decorators used?
* Which modules use CQRS?

⸻

Quality Analysis

* Which modules violate SRP?
* Where is duplicate retry logic?
* Which functions are overly complex?
* Which code is tightly coupled?

⸻

Refactoring Assistance

* What breaks if module X is removed?
* Which utilities are duplicated?
* Which APIs are deprecated?

⸻

Required Technologies

Concern	Recommendation
AST heuristics	custom
Graph analysis	dependency graph
Similarity	embedding clustering

⸻

PHASE 6 — Advanced Software Intelligence (Future)

Goal

Move toward intelligent repository reasoning.

⸻

Questions Answered

High-Level Reasoning

* Explain checkout architecture to new developer.
* Compare authentication across services.
* Suggest best place to add caching.
* Identify risky upgrade areas.
* Find inconsistent validation logic.

⸻

Cross-Repository Intelligence

* Which repos use shared auth SDK?
* Which services still use API v1?
* Which repos depend on common package X?

⸻

Predictive Intelligence

* Which modules are most fragile?
* Which files likely cause outages?
* Which code paths lack observability?

⸻

4. Recommended Local Models

Embeddings

Model	Notes
bge-small-en	best balance
nomic-embed-text	strong semantic retrieval
e5-small	lightweight

⸻

Generation Models

Model	Use
Qwen2.5 1.5B	default
Phi-3 Mini	strong reasoning
Gemma 2B	lightweight
TinyLlama	ultra-fast
DeepSeek Coder 1.3B	code-focused

⸻

5. Critical Design Principle

The system succeeds because:

* retrieval is strong
* indexing is rich
* metadata is structured

NOT because the LLM is large.

This architecture intentionally minimizes dependence on:

* giant context windows
* expensive inference
* cloud reasoning

The ideal flow is:

Repository Intelligence
        ↓
Structured Retrieval
        ↓
Focused Context
        ↓
Small Local Model
        ↓
Concise Answer

⸻

6. Recommended MVP

Absolute MVP

Must Have

* filesystem indexing
* ignore/allow rules
* semantic search
* exact search
* line references
* local embeddings
* incremental indexing

First Valuable Questions

* Where is OpenAI key stored?
* Find yesterday standup report
* Where is retry logic implemented?
* What config uses Redis?
* Which files mention Kafka?

⸻

7. Long-Term Positioning

This is NOT:

* a chatbot over code
* a simple vector search app

It is:

A Local Software Intelligence System

focused on:

* repository understanding
* architectural reasoning
* developer productivity
* operational awareness
* private/offline environments

using:

* micro-LLMs
* deterministic retrieval
* structured indexing
* lightweight reasoning.
