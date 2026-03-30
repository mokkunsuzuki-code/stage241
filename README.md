Stage238: SLSA-aligned Build via Reusable Workflow and Policy

## Overview

Stage238 upgrades the build and attestation model from a single workflow implementation to a **policy-governed reusable workflow architecture**.

This stage ensures that:

- All build provenance is generated through a centralized reusable workflow
- SBOM generation and attestation are standardized
- Build logic is no longer duplicated across workflows
- Supply-chain evidence generation becomes policy-bound

---

## Key Concept

Previous stage:

Stage237  
→ Attestation is generated

Stage238:

→ **Attestation generation is standardized and enforced via reusable workflow + policy**

---

## Architecture

### 1. Caller Workflow (Policy Entry Point)

`.github/workflows/stage238-slsa-policy.yml`

- Defines allowed entrypoint
- Delegates execution to reusable workflow
- Does not implement build logic directly

---

### 2. Reusable Workflow (Policy Core)

`.github/workflows/reusable-slsa-build.yml`

Responsible for:

- Artifact build
- SHA256 digest generation
- SBOM generation (SPDX via Syft)
- Artifact upload
- Build provenance attestation
- SBOM attestation

---

### 3. Policy Document

`docs/slsa_policy.md`

Defines:

- Allowed build path
- Required permissions
- Expected outputs
- Reviewer verification points

---

## Build Outputs

- `stage238-source-bundle.tar.gz`
- `stage238-source-bundle.sha256`
- SPDX SBOM (`.spdx.json`)
- GitHub build provenance attestation
- GitHub SBOM attestation
- Sigstore signature
- Rekor transparency log entry

---

## Security Meaning

This stage does NOT claim:

- Full SLSA compliance
- Complete supply-chain security guarantees

Instead, it provides:

- Governance over how provenance is generated
- Reproducible and auditable build structure
- Standardized attestation pipeline

---

## Local Verification

```bash
python3 tools/verify_stage238_policy.py
./tools/build_stage238_artifact.sh
GitHub Actions Verification

Run:

stage238-slsa-policy

Verify:

Artifact uploaded
SBOM generated
Provenance attestation created
SBOM attestation created
Rekor transparency entry exists
Reviewer Guide

A reviewer should confirm:

Caller workflow only invokes reusable workflow
Reusable workflow uses workflow_call
Attestation steps are centralized
No duplicate build logic exists
Outputs are consistent across runs
Why This Matters

This stage shifts the system from:

"Attestation exists"

to:

"Attestation is generated through a controlled, reusable, policy-defined path"

This improves:

Reproducibility
Auditability
Trust in supply-chain evidence
Next Stage

Stage239:

→ Verification Policy Enforcement

Only artifacts that satisfy policy conditions are accepted.

License

MIT License © 2025 Motohiro Suzuki