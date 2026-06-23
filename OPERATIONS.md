# Operations

The private receptionist is operated as a guarded business automation system.
The operational model favors clarity over cleverness: know what is enabled,
know what can cause side effects, and make rollback simple.

## Hosted Worker

The private system uses a hosted cron-style worker for recurring processing.
The worker checks intake and message queues, applies eligibility rules, updates
state, and performs only the actions currently allowed by environment settings
and policy.

## Safe Environment Flags

Operational behavior is controlled by flags for areas such as dry-run mode,
live customer actions, booking, test allowlists, channel enablement, and logging
mode. The exact values are private, but the public principle is simple: live
behavior must be explicit.

## Gmail Label Testing

Standalone inbox handling is tested through controlled labels and business
aliases. This allows the owner to test real Gmail behavior without giving the
receptionist open-ended authority over the entire inbox.

## Cal.com Integration

The receptionist uses Cal.com as the scheduling provider. Availability and
booking facts are treated as provider state that must be looked up and
revalidated before customer-facing booking actions.

## Manual Review Queue

Review is part of normal operations. The owner can inspect proposed actions,
approve or reject them, and keep the receptionist from guessing in ambiguous
cases.

## Rollback Mindset

The system is designed so operators can stop live behavior by disabling flags
instead of editing source code in an emergency. Hosted jobs should fail closed
when required configuration is missing.

## Staged Rollout

New capabilities move through a staged path:

1. local deterministic tests;
2. side-effect-free scenario testing;
3. owner-only provider tests;
4. hosted dry run;
5. limited live behavior;
6. broader enablement after review.

This keeps production learning tied to explicit safety boundaries.
