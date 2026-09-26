# 2.3.05 — Choosing and configuring load balancers

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

First distinguish application-layer HTTP(S) routing from transport-layer TCP/UDP requirements. Application Load Balancers can route using HTTP attributes such as host/path. Network Load Balancers address supported transport-level cases. Then choose external or internal exposure, global/cross-region or regional scope, and proxy or passthrough behavior as required.

Backend health, frontend configuration, certificate setup, and firewall reachability all matter. Proxy and passthrough designs differ in connection handling and client-IP behavior. Not every load-balancer family supports every protocol, scope, backend type, or Network Service Tier.

## Practical workflow

Write protocol, exposure, region requirements, and backend type; follow the official selection matrix. Configure health checks and backend access, then test the frontend and verify traffic reaches healthy backends.

## Exam trap

A global frontend does not automatically make a single-region backend survive a region failure. An HTTP path-routing requirement rules out generic L4-only routing.

## Check your understanding

**Scenario:** Public requests to /images and /api must reach different backend services using one HTTPS endpoint. Which family fits?

<details>
<summary>Answer and reasoning</summary>

An external Application Load Balancer. Its application-layer routing understands URL paths; a basic transport-layer load balancer does not.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/load-balancing/docs/choosing-load-balancer)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
