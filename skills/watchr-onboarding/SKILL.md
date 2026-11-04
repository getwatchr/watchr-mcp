---
name: watchr-onboarding
description: Start or finish Watchr competitive monitoring from an assistant through the connected Watchr MCP. Use when a person needs an account, OAuth connection, first watchlist, or improvements to existing brands, sources and criteria. Readers can explore without changing configuration.
---

# Watchr onboarding

Help the user start or finish Watchr setup in their assistant. Respond in the user's language and explain product choices in plain language. Reuse any account and watchlists already present.

## Start from the assistant

First check whether Watchr MCP tools are connected and authorized. The standard MCP endpoint is `https://mcp.watchr.ai/mcp`; the Codex/OpenAI profile uses `https://mcp.watchr.ai/mcp/chatgpt`. A skill installation alone does not connect MCP or grant Watchr permissions. If the tools are unavailable, guide the user through the client's own MCP connection and browser OAuth flow. Never ask for a password, token or API key in chat, or simulate a connection.

If the person has no Watchr account, explain that a dedicated Watchr signup page for assistant users is required. **Do not send them to the current multi-step signup wizard or invent a live agent-signup URL.** Offer the dedicated URL only after Watchr has published and verified it. On that page the person creates a minimal account, accepts the terms, and resumes the client's OAuth authorization. The free trial starts when the account is created, not when the first watchlist is configured. The user enters credentials and accepts terms in Watchr's browser UI, never in chat.

Keep the person's stated objective and proposed competitors in the conversation while they sign up; do not treat browser return as proof of account creation or MCP authorization. If signup is abandoned, leave configuration untouched and invite them to resume later. After OAuth returns to the assistant, call `get_workspace_context` before continuing. If authorization is still missing, ask them to finish the client's connection flow; do not guess the account state.

## Read the current setup first

Call `get_workspace_context` to read the effective role, capabilities, limits and existing monitoring groups. Do not infer configuration permission from a job title or from installing this skill. Read `get_watchr_guide` with topic `start`; use `list_watchr_documentation` and the relevant guides when a source, setting or tool needs explanation.

Select the watchlist identified by the user. If there is exactly one, use it automatically. If several could match, ask which label or brands they mean. Read its current state with `get_monitoring_configuration`. Resolve brand identifiers from returned configuration or `list_brands`; never invent IDs.

For an authorized account with no watchlist, ask the administrator for the monitoring objective and competitor domains, then prepare a new watchlist through the MCP preview. Creating a watchlist requires relevance criteria (`custom_prompt`) and at least one domain; never invent domains. Do not create a duplicate when a matching watchlist already exists. If only consultation tools are available, explain that configuration is unavailable in this connection and direct the user to Watchr's settings.

## Complete the useful gaps

Summarize what is already configured. Ask only for information missing from the account and the user's request: which decisions they want the monitoring to inform, important competitors and domains, useful events, sources and exclusions. For an empty watchlist, gather these in conversation. Keep questions focused; do not repeat answers already supplied before signup.

For an administrator with configuration permission, propose only the changes needed for that objective:

- Clarify each brand's activity in its description. Its news-search name and homonym exclusions are separate settings; the search name is not an arbitrary Boolean query.
- Refine the watchlist's relevance criteria, up to 3,000 characters, using desired events and explicit exclusions. Preserve unrelated criteria and other fields.
- Check configured profile URLs, editorial sources and monitored pages. Suggest exact URLs supported by the tools and the available page frequencies; mark unverified candidates clearly. Read the appropriate source guide for limits.
- Explain report cadence and report activation accurately: the current enabled switch controls both preparation of future reports and their email delivery. Source collection continues when reports are disabled.

Use product-level explanations. Do not name internal collection suppliers or invent aggregate source counts. Do not promise independent qualification for shared brands without checking its scope. A criteria change does not automatically reanalyse historical results.

For a reader, explain the existing setup and offer a sourced summary from available insights or reports. Do not preview or apply configuration, enable emails or manage members. If they want an edit, explain that an account administrator must make it.

## Preview, approve, apply

Use `preview_monitoring_changes` with typed actions when available. On the Codex/OpenAI MCP profile, use the matching `preview_<action>` and `apply_<action>` tools instead; never swap the action while applying a preview. Show the affected watchlist or brand, before/after values, effects and any cost information before requesting approval. The preview does not start collection or apply changes. Installing or invoking this skill is not approval of a preview.

Keep report emails off for new MCP-created watchlists. Preserve an existing watchlist's email preference. Include an email-delivery change only when the user explicitly requests it, after checking recipients and cadence. Completing onboarding or asking for a brief does not activate emails. Do not send invitations or change account access as an incidental setup step.

After the user approves the exact preview, call the matching apply tool with its operation ID and `confirmed=true`. If additional connector permission is required, let the user authorize it in the client. A reader role cannot be elevated through connector consent.

If the standard MCP profile returns a Watchr confirmation URL for a price increase or shared configuration, present it and explain the reason. The user confirms on Watchr with their own session. The Codex/OpenAI profile supports existing entitlements only: explain a billing-related refusal without offering a checkout or upgrade link. Never mark payment verified or changes applied just because a link was opened or the user returned to chat; read `get_operation_status`.

An expired or stale preview requires reading the current configuration and preparing a fresh preview. For a partial operation, explain the completed steps and prepare only the remaining changes after rereading state; do not replay the whole operation. Preserve the operation ID when checking progress or retrying an eligible unchanged request.

## Verify and hand back

Reread the configuration after a successful apply and use `diagnose_monitoring` for observable gaps or pending discovery. Distinguish saved settings, collection in progress and actual available results. Do not claim the first collection or report is finished without evidence.

Finish with a short account of the configured brands and criteria, useful remaining gaps, report email state and what to ask next. When results exist, offer a first brief using `search_insights` or `list_reports`; read details when necessary and cite original sources and dates. Distinguish publication dates from detection dates. Empty results do not establish competitor inactivity.
