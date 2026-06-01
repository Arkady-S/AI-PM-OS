Retrieval-Augmented Generation: grounding LLMs in facts via pipeline, chunking, and retrieval economics

**March 2026**

## CORE CONCEPT

RAG is an open-book exam for LLMs. Instead of relying on training memory, the model looks up relevant information before answering. This reduces hallucination, keeps answers current, and enables use of proprietary data.

Retrieval failures are silent. If retrieval misses the right document, the model confidently answers with whatever it did retrieve, or hallucinates. Unlike a 500 error, bad retrieval looks like a normal response. This is the key risk.

## THE RAG PIPELINE

> **Index (offline):** Collect docs > Chunk > Embed > Store in vector DB.
>
> **Retrieve (per-query):** Embed query > Hybrid search (semantic + BM25) > Rerank > Select top-K.
>
> **Generate (per-query):** System prompt + retrieved chunks + user query > LLM > Answer with citations.

## CHUNKING STRATEGY

How you split documents is often more important than which embedding model or vector DB you use. Most RAG problems are chunking problems. Tune this first.

| Size | Tokens | Behavior | Best For |
|---|---|---|---|
| Small | 128-256 | Precise lookup, loses context | Factual Q&A, definitions |
| Medium | 512-1024 | Balanced precision and context | Default for most use cases |
| Large | 1024+ | Preserves context, retrieves noise | Analysis, summarization |

**Overlap:** 10-20% at chunk boundaries prevents losing information at splits. Use paragraph or section boundaries when possible, not arbitrary token counts.

## WHEN TO USE RAG

| Use RAG When | Don't Use RAG When |
|---|---|
| Knowledge changes frequently | Small static corpus (just use long context) |
| Need citations for trust or compliance | Need behavior change (use prompting or fine-tuning) |
| Using proprietary or private data | Creative tasks (RAG adds overhead, no benefit) |
| Corpus too large to fit in context | Cost/latency budget is extremely tight |

## HYBRID SEARCH IS THE DEFAULT

| Method | Finds | Misses | Use |
|---|---|---|---|
| Semantic (vector) | Conceptual matches, paraphrases | Exact terms, IDs, codes | Always include |
| Keyword (BM25) | Exact matches, specific terms | Paraphrases, synonyms | Always include |
| Reranking | Best results from combined set | Nothing new; refines existing | Add when precision matters |

Semantic-only search is almost always worse. Combine both with reciprocal rank fusion, then optionally rerank. This is settled best practice.

## RETRIEVAL BUDGET

Every retrieved chunk is input tokens you're paying for. Retrieval adds cost and latency. State the budget explicitly at design time:

| Element | What You Define | Example |
|---|---|---|
| Top-K | Number of chunks retrieved | 3-5 chunks typical |
| Token budget | Max tokens from retrieval | ~1,500-2,500 tokens per query |
| Latency budget | Max retrieval time | <200ms interactive, <2s async |
| Cost per query | Retrieval + generation cost | Calculate at design time |
| Search method | How retrieval works | Hybrid + reranking |

## PRD ELEMENTS

| Element | What You Define | Example |
|---|---|---|
| Source documents | What's in the knowledge base | Help center, policy docs, product docs |
| Update frequency | How often corpus refreshes | Daily sync, real-time for critical docs |
| Chunking strategy | Size, overlap, structure | 512 tokens, 10% overlap, paragraph bounds |
| Retrieval depth | Top-K and token budget | Top-5 chunks, ~2,500 tokens max |
| Search method | Semantic, keyword, hybrid | Hybrid with reciprocal rank fusion |
| Retrieval eval | How you measure retrieval quality | Recall@5 on 200 query-doc pairs |
| Citation requirements | How model attributes sources | Must cite source for each claim |
| No-result handling | What if retrieval finds nothing | Say "I don't have info on that" |
| Cost/latency budget | Constraints per query | Retrieval <200ms, <$0.01/query |

## COMMON FAILURE MODES

| Failure | Symptom | Fix |
|---------|---------|-----|
| Retrieval miss | Confident wrong answer (right doc not found) | Better chunking, hybrid search, query expansion |
| Retrieval noise | Rambling, unfocused answer (irrelevant chunks) | Smaller chunks, reranking, relevance threshold |
| Stale data | Outdated answer | Increase corpus sync frequency |
| Token overload | Inconsistent behavior, ignored instructions | Reduce top-K, compress chunks |
| No-result silence | Model fabricates when nothing retrieved | Explicit handling: "I don't have info on that" |

→ See: Context Engineering (token budgets, retrieval context)
→ See: Evals & Observability (retrieval eval, golden set)
