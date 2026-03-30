---
applyTo: "kubernetes/**/*.yaml,kubernetes/**/*.yml,.github/workflows/**"
excludeAgent: "coding-agent"
---

# Reviewing Renovate Bot Pull Requests

When reviewing a pull request authored by `renovate[bot]`, follow this checklist.

## 1. Identify the Update Type

- Determine whether the PR is a **major**, **minor**, **patch**, **pin**, or **digest** update.
- Major updates of critical infrastructure packages (cilium, cert-manager, external-dns, ingress-nginx, kube-prometheus-stack) must NOT be auto-merged and require extra scrutiny: check upstream changelogs for breaking changes, API deprecations, and CRD schema changes.
- Patch updates for non-0.x Docker images are treated as security fixes (`fix(security):` commit scope). Confirm the fix is genuine and the image tag/digest is valid.

## 2. Validate the Changed Manifests

- **Image tags and digests**: Verify the new image tag exists and the digest (if pinned) matches the expected tag. Renovate pins digests for Docker images and GitHub Actions per the repo config.
- **Helm chart versions**: For HelmRelease updates, confirm the `chart.spec.version` or `chartRef` points to a valid, published chart version. Check that `values` remain compatible with the new chart version.
- **OCI repository references**: If an OCIRepository `ref.tag` or `ref.digest` is updated, confirm it matches a real published artifact.
- **Grouped updates**: Some packages are grouped (immich images, arr apps, k3s + system-upgrade-controller, GitHub Actions, Python dependencies, Helm charts across clusters). Verify all items in the group are updated consistently and that versions are compatible with each other.

## 3. Review YAML Structure and Kustomization Integrity

- Ensure the YAML is valid and well-formed (no syntax errors, correct indentation).
- If a `kustomization.yaml` file is modified, verify all referenced resources still exist and paths are correct.
- For Flux Kustomization (`ks.yaml`) changes, confirm `spec.path`, `spec.sourceRef`, and `spec.targetNamespace` are correct.
- Variable substitutions using `${VARIABLE_NAME}` should reference known variables from `cluster-settings` ConfigMap or `cluster-secrets` Secret.
- Encrypted secrets (`*.sops.yaml`) should never appear in Renovate PRs. Flag if any are modified.

## 4. Cross-Cluster Consistency

This repo manages two independent clusters: `raspberry` and `turing`. When Renovate updates a Helm chart or shared component:
- Verify both clusters are updated if the same chart is used in both (Renovate groups Helm chart updates across clusters).
- If only one cluster is updated, confirm this is intentional (the component may only exist in one cluster).

## 5. Verify GitHub Actions Workflow Results

Renovate PRs touching `kubernetes/**` trigger these workflows. Check that all have passed:

### Flux Diff (`flux-diff.yaml`)
- Runs on PRs to `main` when `kubernetes/**` is changed.
- Produces a diff of `helmrelease` and `kustomization` resources for both clusters (`raspberry` and `turing`) against the `main` branch.
- Review the diff comments posted on the PR to confirm the changes match expectations: only the intended resources should change, and no unrelated resources should be affected.
- The summary job `Flux Diff Successful` must pass (it fails if any matrix job fails or is cancelled).

### Kubeconform (`kubeconform.yaml`)
- Runs on PRs to `main` when `kubernetes/**` is changed.
- Validates all Kubernetes manifests against their schemas.
- Must pass with no validation errors. Missing schemas are ignored (expected for CRDs), but standard resources must validate.

### Labeler (`labeler.yaml`)
- Runs on all PRs via `pull_request_target`.
- Auto-applies labels based on changed paths (e.g., `cluster/raspberry`, `cluster/turing`, `area/kubernetes`, `app/media`, `app/networking`).
- Verify the correct labels are applied. Renovate also adds its own labels (`type/major`, `type/minor`, `type/patch`, `type/pin`, `type/digest`, and group-specific labels like `renovate/arr-apps`, `renovate/github-actions`).

### Summary
- All three PR-triggered checks (Flux Diff Successful, Kubeconform, Labeler) must be green before approving.
- If any check is failing or pending, do not approve. Suggest the author re-run failed checks or investigate the failure.

## 6. Automerge Eligibility

Cross-check whether the PR should be auto-merged based on the Renovate config:
- **Automerge enabled by default** for: patch updates, digest updates, GitHub Actions updates, and Docker patch updates for non-0.x versions.
- **Automerge disabled** for: major updates of critical infrastructure (cilium, cert-manager, external-dns, ingress-nginx, kube-prometheus-stack).
- If `platformAutomerge` is active and checks pass, the PR will merge automatically. Only flag if the automerge behavior seems incorrect for the update type.
