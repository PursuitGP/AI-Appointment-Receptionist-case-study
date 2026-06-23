# Case Study: Guarded AI Receptionist for Appointment Scheduling

## Business Problem

A coaching business receives appointment inquiries from prospective students,
parents, and existing customers. Responding well is more complex than sending a
friendly email. The receptionist has to understand the customer, offer real
availability, keep track of prior messages, avoid contacting historical leads
unexpectedly, and know when a human should decide.

The core product question was: how much of this workflow can be automated
without giving an AI model unsafe authority over customer communication and
booking actions?

## Product Goal

The goal was to build a receptionist that could:

- detect new appointment inquiries;
- respond with grounded availability;
- understand simple scheduling replies;
- preserve conversation and booking state;
- escalate ambiguous cases;
- support a controlled path from local testing to hosted execution;
- remain reusable for other appointment-based businesses.

The system was designed as guarded automation, not full autonomy. It helps the
business respond faster while preserving operator control over risky or unclear
situations.

## Architecture Summary

The private system is organized around a few durable boundaries:

- **Channel adapters** normalize messages from intake sources such as forms,
  Gmail, provider webhooks, and future SMS or social channels.
- **Identity resolution** decides whether the system knows who is speaking and
  what relationship they have to the student or booking.
- **Operational state** tracks submissions, contacts, conversations, offered
  slots, selected slots, bookings, and review items.
- **Deterministic policy** decides what actions are permitted.
- **Guarded AI** helps interpret language or draft warm responses only within
  facts supplied by the system.
- **Approval gates** route ambiguous or consequential work to the owner.
- **Side-effect adapters** send email, initiate booking actions, and write audit
  records after validation.

This keeps the LLM useful without letting it become the authority for identity,
availability, credits, bookings, or customer-impacting actions.

## Safety Constraints

The workflow was built around several constraints from the beginning:

- Do not contact historical customers unexpectedly.
- Do not treat every email address as a verified identity.
- Do not let a model invent availability or business policy.
- Do not book, cancel, reschedule, or change important state without validated
  context.
- Do not let retries create duplicate customer-facing side effects.
- Do not expose private data in public artifacts.
- Do not rely on a local machine marker as the only source of production truth.

Those constraints shaped the implementation more than the AI layer did. The
hard part was not generating polite text. The hard part was making sure the
system knew when it was allowed to act.

## Development Milestones

1. **Form-based lead intake**

   The first milestone normalized new appointment inquiries from a business
   intake form and connected them to scheduling logic.

2. **Grounded availability**

   The receptionist began offering real appointment windows from the scheduling
   provider instead of generic availability language.

3. **State tracking**

   A lightweight operational store tracked each submission, offered slots,
   selected replies, customer context, and booking state.

4. **Reply handling**

   Gmail replies were correlated back to the correct workflow so clear responses
   could advance and ambiguous responses could be reviewed.

5. **Booking validation**

   Selected slots were revalidated before booking actions to avoid stale or
   duplicate scheduling.

6. **Human approval paths**

   Business policy could require owner approval before consequential actions.

7. **Guarded AI behavior**

   Claude was integrated as a drafting and interpretation helper, with
   deterministic checks retaining ownership of facts and actions.

8. **Hosted rollout controls**

   Railway deployment, dry-run behavior, test allowlists, and preflight checks
   were added to make hosted execution safer.

9. **Channel-independent foundation**

   The architecture began moving beyond form-only intake toward Gmail, provider
   webhooks, SMS, and other appointment-business channels.

## Challenges Solved

### Avoiding Unsafe "AI Magic"

The system does not ask a model to decide what should happen to a customer. It
passes the model bounded tasks and validates the result against deterministic
state and policy.

### Handling Ambiguity

Messages like "that works" are only useful if they can be tied to a specific
conversation and offered slot. If the system cannot confidently resolve the
meaning, it creates a review item instead of guessing.

### Preventing Duplicate Side Effects

Hosted jobs, retries, provider uncertainty, and webhook delivery can all cause
the same event to be seen more than once. The private system uses durable state,
message identifiers, booking request identifiers, and status transitions to make
reprocessing safe.

### Separating Current Customers From New Leads

Existing students, parents, and guardians should not receive new-lead trial
language. The system keeps relationship context separate from generic contact
records so future behavior can respect role and authorization.

### Operating Against Real Business Data

The workflow had to be testable without accidentally emailing or booking real
customers. Dry runs, owner-only tests, deployment gates, and review paths were
treated as product requirements rather than afterthoughts.

## Current Status

The private system is a working, production-oriented receptionist for a chess
coaching workflow. It has been tested with controlled business scenarios and
guarded deployment settings. The architecture is being extended toward a more
general receptionist that can serve other appointment-based businesses.

The public repository is intentionally documentation-only. It is meant to show
engineering judgment, not distribute the business system itself.

## Future Roadmap

The next direction is to make the receptionist less dependent on a single intake
path and more useful across channels:

- channel-independent Gmail handling;
- Cal.com webhook ledger and reconciliation;
- richer existing-client support;
- SMS after consent, identity, and review controls are mature;
- social-media adapters;
- owner dashboard;
- PostgreSQL migration when operational volume justifies it;
- multi-business configuration and packaging.
