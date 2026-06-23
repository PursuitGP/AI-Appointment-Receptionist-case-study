# Testing and Release

The private receptionist system is tested as an automation product, not as a
chat demo. The main risk is not whether it can produce a friendly sentence. The
main risk is whether it performs the correct customer-impacting action under
realistic state, retry, and ambiguity conditions.

## Testing Strategy

### Unit Tests

Unit tests cover deterministic behavior such as message classification, slot
matching, state transitions, authorization checks, and policy decisions. These
tests are fast and local.

### Integration-Style Tests

Integration-style tests exercise larger workflows across intake, scheduling
state, inbound replies, review decisions, and booking preparation. External
side effects are controlled or replaced so tests can run safely.

### Side-Effect-Free Local Console

The private project includes a local scenario console for replaying realistic
email and booking situations without calling Gmail, Cal.com, Claude, or Google
Sheets. This made it easier to test ambiguous replies, existing-client cases,
and customer relationship scenarios quickly.

### Controlled Gmail and Cal.com Testing

Provider integrations are tested through controlled accounts, labels, test
identities, and dry-run settings. The goal is to prove that the system can work
against real provider behavior while preventing accidental outreach to real
customers.

### Regression Suite

Regression tests protect the behaviors that matter most:

- availability wording stays grounded;
- unavailable requested windows are acknowledged honestly;
- clear replies select only previously offered slots;
- ambiguous replies route to review;
- duplicate processing does not create duplicate actions;
- disabled features remain disabled;
- production data does not become test data accidentally.

This public case study does not claim an exact test count because the private
suite is active and changes over time.

## Release Discipline

### Deployment Preflight

Before hosted execution, the private workflow checks required environment and
safety settings. Missing configuration should stop the workflow before customer
side effects can happen.

### Staged Rollout

The rollout path moves from local tests, to side-effect-free scenario runs, to
owner-only provider tests, to hosted dry runs, to controlled live behavior.

### Kill Switches

Live booking and customer-facing sends are controlled by explicit flags. The
safe operating assumption is that new capabilities start disabled until tested
with the owner.

### Release Notes and Handoffs

Operational handoffs include what is enabled, what is disabled, which safety
flags matter, what was verified, and which next action would change production
behavior.
