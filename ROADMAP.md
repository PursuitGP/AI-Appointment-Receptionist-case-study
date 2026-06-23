# Roadmap

The private receptionist began as a chess coaching scheduling workflow and is
being shaped into a reusable receptionist pattern for appointment-based
businesses.

## Current State

- Proven form-based intake for new scheduling inquiries.
- Gmail reply correlation for scheduling conversations.
- Standalone Gmail/business-alias handling implemented behind label, allowlist,
  and mode flags.
- Cal.com availability and booking validation.
- Cal.com webhook receipt, event filtering, idempotent ProviderEvents storage,
  and async booking-ledger reconciliation implemented as a staged foundation.
- Twilio SMS support implemented but gated/disabled pending controlled
  activation.
- Lightweight operational state in Google Sheets with a PostgreSQL migration
  path.
- Guarded Claude interpretation and drafting.
- Owner review paths for ambiguous or consequential actions.
- Hosted execution with controlled rollout flags.

## Current Foundation: Channel-Independent Receptionist

The current foundation is channel-independent enough to normalize form
submissions, Gmail messages, provider events, and gated SMS interactions into
common internal records before identity, policy, review, and side-effect
decisions.

Implemented foundation:

- customer, student, and relationship records;
- review queue execution paths;
- normalized message and interaction records;
- deterministic policy gates around side effects;
- idempotency and retry safety for provider and messaging workflows;
- Railway cron execution and operational handoff patterns.

## Next Milestone: Controlled Channel Activation and Hardening

The next major milestone is controlled activation of the implemented foundation,
not a rewrite into a new architecture.

Key work:

- hosted staging for standalone Gmail/business-alias handling;
- tighter identity resolution and relationship-aware customer context;
- clearer review categories and owner action outcomes;
- staged Cal.com webhook activation after reconciliation checks;
- sender/A2P registration for SMS;
- hosted Twilio webhook staging;
- consent verification and a controlled end-to-end SMS test;
- better separation of private facts from public business facts.

## Cal.com Webhook Ledger

Cal.com event receipt and reconciliation are implemented as a staged foundation.
Incoming events are verified, filtered, stored idempotently, and reconciled into
a booking ledger. Activation remains guarded so provider-side events do not
automatically become customer-facing actions.

## Existing-Client Support

Existing clients need different treatment than new leads. Future work includes
safer support for rescheduling, cancellations, guardian relationships, lesson
credit questions, and owner-approved account changes.

## Twilio and SMS

Twilio SMS is implemented but gated/disabled pending controlled activation. The
foundation includes consent-aware routing, signed inbound and status webhooks,
STOP/START/HELP handling, queued SMS interaction storage, and an async SMS
worker.

Remaining work is sender/A2P registration, hosted webhook staging, consent
verification, and a controlled end-to-end test before any customer-facing SMS
launch. The goal is not to add another notification path casually. The goal is
to make SMS safe for real customer workflows.

## Social-Media Adapters

Future adapters could normalize inquiries from social platforms into the same
receptionist pipeline. The system should not care whether a request began in a
form, email, SMS, or social inbox once identity and authorization are resolved.

## Dashboard

An owner dashboard would make review items, booking status, conversation state,
and operational health easier to inspect without opening raw provider tools.

## PostgreSQL Migration

Google Sheets is useful for early operational visibility and lightweight state.
As volume and query complexity grow, the natural next step is PostgreSQL for
stronger schema control, better concurrency, and more durable audit history.

## Multi-Business Generalization

The long-term product direction is a configurable receptionist for appointment
businesses such as coaching, tutoring, consulting, clinics, studios, and local
services. Business policy, intake channels, provider integrations, approval
rules, and message tone should be configurable without rewriting the core
workflow.
