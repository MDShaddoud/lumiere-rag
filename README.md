# Lumière RAG — Bilingual Salon Knowledge Assistant

A Retrieval-Augmented Generation (RAG) pipeline that lets an AI salon assistant answer customer questions in **Arabic (Syrian dialect) or English**, using only the salon's own knowledge base.

## How it works
1. **Chunking** — the knowledge base is split into one-topic sections
2. **Embeddings** — each chunk is embedded with Gemini (`RETRIEVAL_DOCUMENT`)
3. **Retrieval** — the question is embedded (`RETRIEVAL_QUERY`) and ranked against all chunks by cosine similarity (NumPy)
4. **Grounded answer** — the top 3 chunks are passed to the LLM, which answers only from them
5. **Safety** — if the answer isn't in the knowledge base, it replies "I'll check with the team" instead of guessing

## Engineering details
- Language routed by code (Unicode detection), not left to the model
- Model fallback with retry and exponential backoff for 503/429 errors
- API key stored in Colab Secrets, never in code

## Test results
| Question | Retrieved | Answer |
|---|---|---|
| How long does keratin last? | Keratin Treatment (0.71) | 3–5 months |
| بقدر اعمل كيراتين وانا حامل؟ | Keratin Treatment (0.73) | Consult your doctor first |
| Do you have parking? | Location and parking (0.71) | Free, 3 hours |
| Do you do hair extensions? | No clear match (0.66) | "I'll check with the team" |

Part of a larger n8n booking agent (Telegram + Airtable) built by Mahmoud Shaddoud.
