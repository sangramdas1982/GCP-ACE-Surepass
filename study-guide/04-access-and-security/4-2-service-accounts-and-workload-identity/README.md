# 4.2 — Service accounts and workload identity

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Separate the identity running an application from the human deploying it. Prefer short-lived credentials and precise trust relationships instead of distributing private keys.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. User-managed accounts and Google-managed service agents](01-create-service-accounts.md)
- [ ] [02. Least privilege for workload identities](02-least-privilege-service-accounts.md)
- [ ] [03. Assigning service accounts to resources](03-attach-service-account.md)
- [ ] [04. Permissions held by versus permissions on a service account](04-service-account-policy.md)
- [ ] [05. Service account impersonation](05-impersonation.md)
- [ ] [06. Short-lived access tokens and ID tokens](06-short-lived-credentials.md)
- [ ] [07. Google Cloud identities for GKE applications](07-gke-workload-identity.md)
- [ ] [08. Workload Identity Federation for external workloads](08-external-workload-federation.md)

## Worked study exercise: Separate deployment and runtime identities

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Create a lab service account only if your sandbox permits it; do not create a private key.
2. Draw the deployer's actAs permission on that account and the account's read permission on one bucket.
3. Describe how short-lived impersonation differs from attaching the account to a VM.
4. Compare external CI workload federation, human workforce federation, and GKE workload federation.
5. Write two tests: one required operation should succeed; one unrelated operation should be denied.

**Success evidence:** distinguish permissions on the service account from permissions held by it, and access tokens from audience-specific ID tokens.

**Cleanup:** remove test bindings and the unused lab account after confirming no running resource relies on it.

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
