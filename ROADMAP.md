# Roadmap

The private receptionist began as a chess coaching scheduling workflow and is
being shaped into a reusable receptionist pattern for appointment-based
businesses.

## Current State

- Form-based intake for new scheduling inquiries.
- Gmail reply handling for scheduling conversations.
- Cal.com availability and booking context.
- Lightweight operational state in Google Sheets.
- Guarded Claude interpretation and drafting.
- Owner review paths for ambiguous or consequential actions.
- Hosted execution with controlled rollout flags.

## Next Milestone: Channel-Independent Gmail

The next major milestone is to make standalone Gmail handling mature enough for
existing customers, parents, guardians, and new leads without depending on a
form submission as the starting point.

Key work:

- stronger identity resolution;
- relationship-aware customer context;
- clearer review categories;
- safer support for existing-client questions;
- better separation of private facts from public business facts.

## Cal.com Webhook Ledger

Provider events should be captured idempotently and reconciled into a booking
ledger. This helps the receptionist understand provider-side changes without
turning every provider event into an automatic customer conversation.

## Existing-Client Support

Existing clients need different treatment than new leads. Future work includes
safer support for rescheduling, cancellations, guardian relationships, lesson
credit questions, and owner-approved account changes.

## Twilio and SMS

SMS is a future channel to expand only after consent, identity, review, and
logging controls are strong enough. The goal is not to add another notification
path casually. The goal is to make SMS safe for real customer workflows.

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
