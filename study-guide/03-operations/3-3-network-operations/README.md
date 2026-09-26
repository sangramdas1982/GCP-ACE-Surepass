# 3.3 — Network operations

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

A working network requires usable addresses, matching routes, permitted traffic, correct DNS, and the appropriate egress mechanism. Verify one layer at a time.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Expanding a subnet IPv4 range](01-resize-subnets.md)
- [ ] [02. Static internal and external IP addresses](02-static-addresses.md)
- [ ] [03. Custom static routes](03-static-routes.md)
- [ ] [04. Cloud DNS, Cloud NAT, and Private Google Access](04-dns-nat.md)
- [ ] [05. Operating firewall rules and policies](05-firewall-operations.md)

## Worked study exercise: Operate addresses, routes, and DNS

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. List lab subnets, routes, addresses, and firewall rules using the command reference.
2. Calculate how /24 differs from /23 and identify a nonoverlapping expansion on paper.
3. Inspect a DNS result and explain public versus private zone visibility.
4. Diagnose the fictional case: a private VM reaches Google APIs but cannot reach a public package repository.
5. Identify where a NAT gateway, route, firewall rule, or DNS correction belongs; avoid changing all four blindly.

**Success evidence:** correctly separate name resolution, next-hop selection, permission, and address translation.

**Cleanup:** release only unused lab static addresses after checking dependencies; remove any temporary network rules you created.

## How to answer this subsection's scenarios

1. Identify the concrete objective and every hard constraint.
2. State the resource and identity involved; separate configuration, permission, and connectivity failures.
3. Eliminate options that violate a requirement before comparing cost or convenience.
4. Prefer the simplest supported solution that satisfies all stated requirements; a managed service is not automatically correct if it lacks a required capability.
5. Explain why the most tempting alternative fails. Record uncertain answers in the [mistake log](../../MISTAKE-LOG.md).

## Completion check

- [ ] I can explain every linked topic in my own words.
- [ ] I can complete the exercise or explain its expected observations.
- [ ] I can distinguish the services/controls commonly confused here.
- [ ] I can answer the topic scenarios without relying on remembered wording.
