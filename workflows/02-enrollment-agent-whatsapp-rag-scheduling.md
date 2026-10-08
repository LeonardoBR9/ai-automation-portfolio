# 02 - WhatsApp Enrollment Agent (RAG + Scheduling + Follow-up Cadence)

**File:** [`02-enrollment-agent-whatsapp-rag-scheduling.json`](02-enrollment-agent-whatsapp-rag-scheduling.json) - 238 nodes (40 sticky notes), inactive by default.

Same ingestion/debounce/identity backbone as workflow 01, extended for a sales process that ends in an appointment and a signed document.

## Flow (differences from 01)

1. **Two lead segments** - a router agent chooses between two qualifier agents (`OFFER_X` and `offer_y`) based on conversation context and lead profile; product specialists with media tools are also available.
2. **Scheduling tools** - `criar_reuniao`, `cancelar_reuniao` and `reagendar_reuniao` call a scheduling sub-workflow, and the agent only confirms after the tool succeeds.
3. **Follow-up cadence by counter** - a 5-minute scheduler selects idle chats within a business-hours window and routes by attempt count (10 min / 20 min / 45 min), using three different LLM prompts and a fixed final message.
4. **Document step** - a contract link is sent once; the flow records `contract_*_sent_at`, avoids resending, and sends a reminder instead.
5. **Social-proof media** - a marker injected into the reply sends a testimonial video exactly once per chat.
6. **Media delivery sub-flow** - Drive folders are listed, files downloaded, converted to base64 and posted to the WhatsApp gateway with a delay between sends.
7. **RAG training pipeline** - Drive folder -> file-type switch (PDF / CSV / Excel / text) -> extract -> split -> embeddings -> vector store, with deletion of stale rows; a separate training agent writes Q&A pairs into a Google Sheet.
8. **Lead alerting** - new leads and qualification reports are posted to an internal group.

## Techniques worth noting

- Idempotency flags in Postgres (`*_sent_at`, `followup_count`, `last_followup_sent_at`) so scheduled flows never double-send.
- Business-hours guard in SQL (`AT TIME ZONE`) on the follow-up selector.
- Tool descriptions written so the model confirms bookings only after a successful tool result.
- Separate models for chat, review and follow-up generation.

## Manual-review notes (what was changed)

- All prompts (qualifier, specialist, router, follow-ups, lead report, classifier) were replaced with generic English skeletons; the original program description, eligibility rules, address and persona are removed.
- Contract and testimonial links, Drive folder/file ids, the sheet id, the sub-workflow id and the media-gateway endpoint were replaced with `https://example.invalid/...` or `PLACEHOLDER_ID`.
- API keys and bearer headers in HTTP nodes were replaced with `<REDACTED>`.
- Client and product names were replaced with `Client B`, `OFFER_X` / `offer_y` and `Product A-D`; internal ticket references in code comments were deleted.
- `Execute Workflow Trigger` is referenced by one expression but belongs to a sub-workflow that is not part of this export.
