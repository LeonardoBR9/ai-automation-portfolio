# 01 - WhatsApp Support / Qualification Agent (RAG)

**File:** [`01-support-agent-whatsapp-rag.json`](01-support-agent-whatsapp-rag.json) - 199 nodes (39 sticky notes), inactive by default.

A multi-agent conversational flow for WhatsApp. One webhook receives every event; the flow normalizes identity, keeps human takeover safe, debounces bursts of messages, routes to a specialist agent and sends the reply back through Evolution API.

## Flow

1. **Webhook + normalizer** - accepts Evolution API events (and optionally WAHA via a normalizer node). Groups and own messages are dropped.
2. **Identity resolution** - maps `@lid` identifiers to phone JIDs, upserts an identity table and reconciles unknown ids from history.
3. **Control routes** - a training keyword, owner numbers and the normal path (`Rotas`); human takeover pauses the AI and a "finished" keyword re-enables it after a wait.
4. **Debounce** - Redis list + wait + compare so only the last execution of a burst continues.
5. **Media** - audio transcription and image description feed the same turn.
6. **Router** - an LLM "sector manager" (or the TypeSafe AI router node) writes the chosen sector to the customer table.
7. **Agents** - generic qualifier, commercial next-step agent and four product-specialist agents (`Product A-D`), each with a *think* tool, Postgres chat memory and a RAG tool; media tools return photos/videos.
8. **Delivery** - a structured-output chain splits the reply into max 3 messages; `[MEDIA: url|type|description]` markers are extracted and sent as image/video.
9. **Persistence & reporting** - Supabase/Postgres tables for customers, chats and messages; a text classifier decides when a chat is closed and an LLM analyst posts a lead report to an internal group.
10. **Follow-up** - a 5-minute scheduler selects idle chats and sends up to two LLM-written nudges.
11. **Bootstrap utilities** - manual-trigger nodes create the tables and the pgvector `documentos_conhecimento` store / `match_documents` function (and wipe them for tests).

## Techniques worth noting

- Debounce with Redis so one answer covers a burst of short messages.
- Human-takeover pause with automatic context injection when the AI resumes.
- Sanitized, parameter-safe SQL builders in Code nodes (quote stripping, regex validation of ids) in front of Postgres queries.
- Two interchangeable WhatsApp providers behind one normalized event shape.

## Manual-review notes (what was changed)

- All agent, chain, classifier and router prompts were replaced with generic English skeletons; client persona, product facts, prices, regions and eligibility rules are gone.
- Owner phone numbers, internal group ids, the webhook path, the closing/protocol code and the training keyword were replaced with placeholders.
- The service-keyword detector in the `Dados` node was replaced with an illustrative `KEYWORD_A -> Service A` mapping.
- Table/column names (Portuguese) were kept since they are not sensitive; node labels stay in the original working language.
