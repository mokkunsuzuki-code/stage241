# Stage239: Verification Policy Enforcement

MIT License © 2025 Motohiro Suzuki

## Overview

Stage239 upgrades the project from **attestation generation** to **verification policy enforcement**.

In earlier stages, the build produced provenance and SBOM attestations as verifiable evidence.
In this stage, the workflow goes further:

- build provenance attestation must exist
- SBOM attestation must exist
- both attestations must verify successfully
- the signer workflow must match the expected reusable workflow
- the artifact identity must match the expected subject
- the SBOM predicate type must be SPDX 2.3

If these conditions are not satisfied, the build is rejected.

This means the repository no longer says only:

> “we created attestations”

It now enforces:

> “we do not accept builds unless the attestation policy is satisfied”

That is the core value of Stage239.

---

## Why this stage matters

This stage changes the security posture from **evidence production** to **admission control**.

### Before
- build artifacts were generated
- attestations were attached
- evidence existed

### After
- evidence is not enough by itself
- verification is mandatory
- policy mismatch causes workflow failure

This is a meaningful step toward stronger supply-chain security governance.

---

## Security meaning

Stage239 introduces a simple but strong policy:

1. provenance must exist
2. SBOM must exist
3. provenance must verify
4. SBOM must verify
5. expected signer workflow must match
6. expected artifact subject must match
7. SBOM predicate type must be SPDX 2.3

If any one of these checks fails, the CI run fails.

This makes the project stricter, clearer, and more externally auditable.

---

## Repository structure

```text
.github/workflows/
  slsa-governed-build.yml
  stage239-verification-policy.yml

docs/
  verification_policy.md

tools/
  build_stage239_artifact.sh
  check_attestation_policy.py

out/
  policy/
Key files
.github/workflows/slsa-governed-build.yml

Reusable workflow that:

checks out the repository
builds the source bundle artifact
generates the SPDX SBOM
uploads the build artifact and SBOM
creates provenance attestation
creates SBOM attestation
.github/workflows/stage239-verification-policy.yml

Main workflow that:

calls the reusable governed build workflow
downloads the generated artifact and SBOM
verifies provenance attestation
verifies SBOM attestation
enforces the verification policy
uploads policy evidence
tools/build_stage239_artifact.sh

Builds the source bundle from an explicit file list:

README.md
tools/
docs/
.github/

This avoids unstable archive behavior in CI.

tools/check_attestation_policy.py

Checks whether the verification result satisfies the expected policy.

It validates:

attestation presence
predicate type
artifact subject
repository identity
signer workflow identity

If policy checks fail, it exits non-zero.

Workflow logic

The Stage239 flow is:

Build
↓
Generate provenance attestation
↓
Generate SBOM attestation
↓
Verify provenance attestation
↓
Verify SBOM attestation
↓
Check policy conditions
↓
Accept or reject build

More concretely:

Artifact
↓
Attestation
↓
Verification
↓
Policy
↓
Admission / Rejection
Local build

You can test the artifact build locally:

chmod +x tools/build_stage239_artifact.sh
./tools/build_stage239_artifact.sh

Expected output:

[OK] built: stage239-source-bundle.tar.gz
GitHub Actions

The main workflow is:

stage239-verification-policy

The reusable workflow is:

slsa-governed-build

A successful run means:

the source bundle was built
the SBOM was generated
provenance attestation was created
SBOM attestation was created
both were verified
policy checks passed
Evidence produced

Stage239 produces policy-oriented evidence such as:

source bundle artifact
SPDX SBOM
provenance verification result
SBOM verification result
policy result JSON

This stage is important because the evidence is not only generated, but also used to make an accept/reject decision.

What changed from Stage238
Stage238
created attestations
moved toward governed builds
Stage239
verifies the attestations
enforces explicit acceptance conditions
rejects builds that do not satisfy policy

So Stage239 is not just “more evidence”.

It is a move toward enforced verification policy.

External review value

For an external reviewer, Stage239 shows that the repository now supports:

reproducible evidence production
verifiable attestations
policy-based acceptance control
explicit rejection on mismatch

This is stronger than a repository that only publishes artifacts.
It demonstrates that supply-chain evidence is operationally enforced.

Limitations

Stage239 is a practical enforcement step, not a full trust framework.

For example:

it does not solve all trust bootstrapping questions
it does not replace broader organizational policy systems
it focuses on repository-level CI enforcement

Still, it is a meaningful and concrete improvement in security engineering practice.

License

MIT License © 2025 Motohiro Suzuki

See LICENSE
 for details.
EOF