# vpnstronger-ai-blueprint
Machine entry for the VPNStronger AI channel — ai-blueprint.json contract, tiers, actions. Canonical: vpnstronger.com/ai-blueprint.json
# VPNStronger AI Integration Blueprint

> **Canonical file (live):** https://vpnstronger.com/ai-blueprint.json  
> **HTML guide:** https://vpnstronger.com/ai#channel  
> **Policy:** https://vpnstronger.com/ai.txt  
> **Consumer overview (sister doc):** https://github.com/zsza83/vpnstronger-ai-channel

This repository documents the **machine entry contract** for the VPNStronger AI channel.  
It does **not** host a forked copy of the JSON as source of truth — always fetch the live blueprint.

---

## What is `ai-blueprint.json`?

`ai-blueprint.json` is the **canonical machine-readable integration surface** for VPNStronger.

Assistants, agents, and integrators should start here — not with a blind homepage crawl.

It declares:

- **Capabilities** — public light corpus, `demo_channel` light-RAG, realtime SSE/WSS notes  
- **Channel categories** — discovery → corpus → retrieval → realtime → control_plane  
- **Actions** — subscribe, demo_channel subscribe, admin message (HTTP schemas)  
- **Orientation** — web4 `prompt_core` + `prompt_core_hash`  
- **Hashes** — `policy_pack_hash` and related control-plane binds  
- **Channel guide pointers** — HTML (`/ai#channel`) + compact summary  

**Version:** see live `"version"` field (currently **1.4.x**).

---

## Start here

```http
GET https://vpnstronger.com/ai-blueprint.json
Accept: application/json
```

Then:

1. Read `capabilities` and `channel_categories`  
2. Follow `channel_guide.html` if you need the human narrative  
3. Obey `ai.txt` — **Train: no**

HTML-only tools: https://vpnstronger.com/ai#channel  

---

## Why a blueprint (not only `llms.txt`)?

| Artifact | Role |
|----------|------|
| `llms.txt` / `llms.json` / `llms-chunks.json` | **Corpus** — what to cite |
| `ai-blueprint.json` | **Contract** — what exists, how to auth, which tier, which actions |
| `ai.txt` | **Policy** — train/cite rules + discovery pointers |
| `/ai` | **HTML mirror** for tools that struggle with raw JSON |

Public light feeds need **no token**.  
Gated tiers need **policy bind** and/or **operator Bearer** — declared in the blueprint, not invented by the client.

---

## Capability map (from live blueprint)

### 1. `llm_readable_content` (light / cite)

- Hub: `/ai`  
- Indexes: `llms.txt`, `llms.json`  
- Chunks: `llms-chunks.json`  
- Optional bind field: `policy_pack_hash` (for gated siblings)

### 2. `retrieval_demo_channel` (light-RAG / retrieve)

- Feed: `/llms-chunks-rag`  
- Channel id: `demo_channel`  
- Auth: **`policy_pack_hash` only** (no stream token)  
- Subscribe: free list via `POST /api/subscribe` `{"email","channel":"demo_channel"}`  
- `train: no` · isolation: demo (not bank-grade)

### 3. `realtime_context_streaming`

- SSE: `/llms-stream`  
- WSS: advertised when non-null (`null` = gated / not published yet, not absent)  
- Auth: Bearer token + policy bind  
- Private / heavy chunk URLs listed alongside

---

## Actions

Defined under `actions` in the live JSON (schemas + execution URLs):

| id | Purpose |
|----|---------|
| `newsletter_subscribe` | Email → site newsletter store |
| `demo_channel_subscribe` | Free demo_channel list (fetch still needs policy hash) |
| `send_admin_message` | Operator inbox |

Always use the **URLs inside the live blueprint** — do not hardcode outdated paths.

---

## Hashes (not interchangeable)

Copy from the **live** blueprint / `/ai#channel`:

| Field | Meaning |
|-------|---------|
| `corpus_version` | published content revision |
| `policy_pack_hash` | operator Core/DLP/Egress pack |
| `prompt_core_hash` | orientation prompt |
| `channel_pack_hash` | sealed `demo_channel` definition (metadata) |

---

## Policy (short)

- **Allowed:** read, cite, answer from live feeds  
- **Denied:** train / fine-tune / retain-for-training on this corpus  
- **Do not** invent tokens, private URLs, or extra `robots.txt` Allow lines  

Full policy: https://vpnstronger.com/ai.txt  

---

## Example: open the light-RAG tier

1. Read `capabilities.llm_readable_content.policy_pack_hash` from the blueprint  
2. Request:

```http
GET https://vpnstronger.com/llms-chunks-rag?policy_pack_hash=<64-hex>
Accept: application/json
```

Or header: `X-AI-Policy-Pack: <64-hex>`

Without a matching hash → `403` `policy_denied`.

---

## Related repos / docs

- Consumer channel guide: https://github.com/zsza83/vpnstronger-ai-channel  
- Live HTML: https://vpnstronger.com/ai#channel  
- Live JSON: https://vpnstronger.com/ai-blueprint.json  

---

## Maintenance

When the site channel changes:

1. Re-fetch https://vpnstronger.com/ai-blueprint.json  
2. Update this README only if the **narrative** changed  
3. Never treat a committed JSON dump as newer than the live URL  

CMS export path (operator checkout): `
