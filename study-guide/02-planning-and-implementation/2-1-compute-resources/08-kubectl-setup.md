# 2.1.08 — Installing and configuring kubectl

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

The gcloud CLI manages Google Cloud resources; kubectl talks to a Kubernetes API server. To operate a GKE cluster, kubectl needs cluster connection information, credentials, and the appropriate authentication plugin. A kubeconfig context identifies the cluster, user, and optional namespace.

Authentication does not guarantee Kubernetes authorization. Google Cloud IAM and Kubernetes RBAC participate in GKE access, while network reachability controls whether the API endpoint is accessible at all. A command using the wrong current context can target a different cluster than intended.

## Practical workflow

Use the supported kubectl and `gke-gcloud-auth-plugin` installation method for your environment. Run `gcloud container clusters get-credentials CLUSTER_NAME --location=LOCATION --project=PROJECT_ID`, then inspect `kubectl config current-context` and `kubectl get namespaces`.

## Exam trap

A connection timeout suggests an endpoint/network issue; a Forbidden response suggests authorization. Do not solve every failure by granting cluster-admin.

## Check your understanding

**Scenario:** kubectl worked yesterday, but today it displays another application environment. What should you check first?

<details>
<summary>Answer and reasoning</summary>

The current kubeconfig context and namespace. Fetching credentials or switching contexts can change the target without changing the application itself.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
