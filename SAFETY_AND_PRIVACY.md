# Safety and Privacy

Safety is the central engineering theme of the private receptionist system.
Because it can contact customers and interact with booking workflows, the system
is designed to fail closed and escalate uncertainty.

## Private Data Boundary

This public repository does not include:

- production source code;
- raw customer communications;
- names, phone numbers, private emails, or booking records;
- OAuth token files, API keys, secrets, provider identifiers, or Sheet IDs;
- raw Railway logs or screenshots containing private operational data.

Public documentation uses sanitized descriptions and conceptual architecture
only.

## Fail-Closed Flags

Production behavior is controlled through explicit environment flags. If a
required flag, credential, token, allowlist, or deployment setting is missing,
the safe behavior is to stop or run without side effects.

This protects against accidental live actions during local development, hosted
deployment, and credential rotation.

## Test Mode and Dry Run

The private system supports controlled test behavior so the owner can exercise
the workflow without contacting unrelated customers. Dry-run mode allows the
workflow to validate decisions and produce logs without sending real outbound
messages or creating live booking side effects.

Owner-only testing also prevents historical production data from being treated
as newly eligible work.

## Owner Approval

Human approval is required for ambiguous, risky, or policy-sensitive actions.
The review path is part of the product, not an exception path. Examples include:

- unknown or conflicting identity;
- unclear scheduling replies;
- cancellation or rescheduling requests;
- customer relationship changes;
- sensitive lesson-credit or account questions;
- unsupported requests outside the receptionist's authority.

## Prompt-Injection Boundary

Customer messages are treated as untrusted input. A message can ask, threaten,
or instruct the system, but it cannot change policy. The model may classify or
draft from approved context, while deterministic code decides which facts and
actions are available.

## Idempotency

The system is designed for retries. Hosted jobs can run again, provider
responses can be uncertain, and webhooks can be delivered more than once.
Durable state and request identifiers keep repeated processing from becoming
repeated side effects.

Protected areas include:

- outbound email sends;
- inbound reply processing;
- selected-slot transitions;
- booking requests;
- provider event ingestion;
- review queue actions.

## Review Queue and Human Escalation

When the system does not have enough confidence or authority, it creates a
review item. This keeps the receptionist useful without pretending every
customer message can be automated safely.

The review queue also creates an operational audit trail: what happened, what
was proposed, what was approved, and what action followed.

## Public Repository Hygiene

This repository is intended for public sharing. It should be reviewed before
publication for:

- accidental source-code copy;
- credentials or provider IDs;
- private customer records;
- raw screenshots;
- real emails or phone numbers;
- operational instructions that would expose the live business setup.
