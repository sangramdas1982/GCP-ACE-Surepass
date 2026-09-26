# 2.2.08 — Spanner: distributed relational transactions

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Spanner combines relational data and transactions with horizontal scaling and supported regional/multi-region configurations. It is relevant when the workload needs relational consistency at a scale or geographic availability that calls for distributed database infrastructure.

Distribution does not eliminate design tradeoffs. Schema, key design, indexes, query patterns, capacity, and client behavior influence performance. Multi-region replication can improve availability and geographic access while changing latency and cost. An ordinary small relational application does not automatically need Spanner simply because high availability is mentioned.

## Practical workflow

Identify scale, consistency and geographic requirements; select a supported instance configuration/capacity model and SQL dialect; design schema/keys; load representative data; and measure queries and transactions. Include backups and application retry behavior.

## Exam trap

Distributed relational support does not mean every query is cheap. Avoid choosing Spanner solely as a fashionable replacement for a straightforward small Cloud SQL workload.

## Check your understanding

**Scenario:** A globally deployed application requires strongly consistent relational transactions and horizontal scale. Which service is a strong candidate?

<details>
<summary>Answer and reasoning</summary>

Spanner. BigQuery is analytical, and Firestore uses a document-oriented model rather than serving as a direct relational replacement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/spanner/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
