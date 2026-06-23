# AI Appointment Receptionist Case Study

This repository is a public technical case study for a private AI receptionist
system built for an active chess coaching business.

The private system handles appointment intake, scheduling conversations,
availability lookup, reply interpretation, booking context, customer state, and
human review flows. This public repository does not contain the production
source code. It documents the product, architecture, safety model, testing
strategy, and rollout discipline behind the system in a form that can be shared
with recruiters, hiring managers, engineers, and potential clients.

## Why This Exists

The original project was built to solve a real operating problem: appointment
inquiries require fast, warm responses, but scheduling involves state,
availability, identity, customer history, and careful handling of ambiguous
messages. A simple chatbot would be unsafe. A manual inbox would not scale.

The result is a guarded AI receptionist: language models can help interpret and
draft, while deterministic application code owns identity, policy, state,
booking validation, external actions, and escalation.

## What I Built

- A stateful appointment receptionist for a real service business.
- Intake handling from Google Forms and Gmail, with a path toward additional
  channels.
- Cal.com availability and booking workflows with revalidation before action.
- Google Sheets backed operational storage for contacts, submissions,
  conversations, bookings, and review queues.
- Guarded Claude drafting and interpretation behind deterministic checks.
- Human approval paths for ambiguous, risky, or consequential actions.
- Idempotency protections against duplicate sends, duplicate processing, and
  duplicate booking attempts.
- Production-oriented environment gates, dry-run behavior, test modes, and
  deployment preflight checks.
- A staged roadmap toward a reusable receptionist platform for appointment
  based businesses beyond chess coaching.

## Engineering Highlights

- **LLM as assistant, not authority:** AI can propose wording or classify
  language, but it cannot directly book, cancel, reschedule, mutate customer
  facts, or bypass policy.
- **Fail-closed operations:** production behavior is locked behind explicit
  environment flags, owner-only test controls, and deployment checks.
- **Stateful automation:** each workflow keeps durable identity, contact,
  conversation, and booking state rather than relying on a stateless prompt.
- **Idempotent side effects:** retries are designed to avoid duplicate emails,
  repeated reply handling, and duplicate booking requests.
- **Human review by design:** ambiguous identity, negative replies, unsupported
  requests, and high-impact changes route to review instead of forcing
  automation.
- **Privacy-aware public artifact:** this repository explains the engineering
  without exposing source, secrets, customer records, private IDs, raw logs, or
  operational credentials.

## What This Demonstrates to Employers

This project demonstrates practical product engineering in the messy middle
between prototype and production:

- designing AI features around bounded responsibility;
- integrating third-party APIs without making them the system of record;
- protecting customers from unintended automation;
- building staged deployment controls for live business data;
- separating deterministic policy from probabilistic language output;
- using lightweight infrastructure while preserving a migration path;
- communicating technical tradeoffs clearly enough for operators and engineers.

## Technology and Concepts

The private implementation uses Python, Google APIs, Gmail, Google Sheets,
Cal.com, Claude via Anthropic, Railway cron workers, local regression tests, and
side-effect-free scenario testing. The important concepts are broader than the
specific stack: channel adapters, identity resolution, state machines,
idempotency, policy gates, review queues, deployment preflight, and safe
rollout.

## Current Status

The private system has been developed and tested against real business
workflows with guarded deployment controls. It supports controlled scheduling
automation for chess coaching and is being extended toward a channel-independent
receptionist model. Some future channels and platform features remain roadmap
items, including broader SMS usage, social-media adapters, a dashboard,
database migration, and multi-business packaging.

## Private Source Notice

The production source remains private because it operates an active business
workflow and may contain proprietary implementation details, integration
structure, deployment configuration, and customer-adjacent operational logic.

This public repository intentionally excludes:

- production source code;
- secrets, tokens, OAuth files, API keys, private Sheet IDs, and raw logs;
- customer names, emails, phone numbers, message history, or booking records;
- instructions that would let someone operate the real business automation.

## Documentation

- [Case Study](CASE_STUDY.md)
- [Architecture](ARCHITECTURE.md)
- [Safety and Privacy](SAFETY_AND_PRIVACY.md)
- [Testing and Release](TESTING_AND_RELEASE.md)
- [Operations](OPERATIONS.md)
- [Roadmap](ROADMAP.md)
- [Assets Guide](assets/README.md)
