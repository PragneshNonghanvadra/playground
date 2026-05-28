# Failure Mode: <Name>

## System under consideration

What is being analyzed.

## Failure description

What goes wrong, in plain language.

## Trigger

What causes this failure? (load, bad input, dependency outage, configuration drift, race, deploy, etc.)

## Symptoms

What does it look like from outside? (errors, latency spike, silent corruption, partial responses, queue backlog, etc.)

## Detection

How would we notice it? Logs, metrics, traces, alerts, customer reports. State the latency from cause to detection honestly.

## Blast radius

What does this affect? Users, regions, tenants, downstream systems.

## Mitigation

Short-term: how to stop the bleeding.

Long-term: design changes that prevent recurrence.

## Test plan

How can this failure be reproduced safely? (chaos test, fault injection, load test, replay).

## Related topics

Links to topic notes that explain the underlying mechanics.

## Lessons

What general lesson does this teach about designing systems?
