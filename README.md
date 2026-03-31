# Stage240: External Verification Gate

MIT License © 2025 Motohiro Suzuki

## Overview

Stage240 extends verification policy enforcement beyond GitHub Actions.

Stage239 established CI-based verification policy enforcement:
builds were accepted or rejected inside CI based on provenance and SBOM attestation checks.

Stage240 goes one step further.

This stage introduces an external verification gate so that the same policy can be enforced outside CI.

This means verification is no longer limited to:

- "GitHub Actions says pass"

It now supports:

- "anyone can verify and enforce the same policy independently"

That is the core value of Stage240.

---

## Why this stage matters

This stage upgrades the trust model from CI-local enforcement to externally reproducible enforcement.

### Before
- CI generated attestations
- CI verified attestations
- CI rejected policy mismatches

### After
- the same verification logic can run outside CI
- third parties can enforce the same policy independently
- trust is no longer bound only to the repository's own workflow

This is an important step toward externally auditable verification.

---

## Security meaning

Stage240 adds three core elements:

1. a policy definition file
2. an external verification CLI
3. an external pass/fail gate

The policy requires:

- provenance attestation
- SBOM attestation
- repository identity match
- signer workflow match
- expected artifact name match
- SPDX 2.3 SBOM predicate

If any of these checks fail, verification fails.

This means the project no longer depends only on CI-internal policy enforcement.
The same decision can now be reproduced externally.

---

## Repository structure

```text
.github/workflows/
  slsa-governed-build.yml
  stage240-external-verification.yml

docs/
  external_verification.md

policy/
  policy.yaml

tools/
  build_stage240_artifact.sh
  verify_external_policy.py

out/
  external_verification/
Key files
policy/policy.yaml

Defines the external verification policy, including:

required attestation types
accepted predicate types
repository identity
signer workflow identity
expected artifact name
tools/build_stage240_artifact.sh

Builds the Stage240 source bundle artifact:

README.md
LICENSE
tools/
docs/
.github/
policy/

This uses an explicit file list to avoid unstable archive behavior.

tools/verify_external_policy.py

Verifies the artifact outside CI using GitHub attestation verification.

It checks:

artifact presence
artifact name
provenance verification
SBOM verification
signer workflow identity
repository identity

It writes external verification evidence to:

out/external_verification/
.github/workflows/slsa-governed-build.yml

Reusable workflow that:

checks out the repository
builds the source bundle
generates the SPDX SBOM
uploads the artifact and SBOM
creates provenance attestation
creates SBOM attestation
.github/workflows/stage240-external-verification.yml

Main workflow that:

calls the reusable governed build
downloads the built artifact
downloads the SBOM
runs the external verification CLI
uploads external verification evidence
Workflow logic

The Stage240 flow is:

Build
↓
Generate provenance attestation
↓
Generate SBOM attestation
↓
Download artifact outside build job
↓
Run external verification CLI
↓
Pass / Fail

More conceptually:

Artifact
↓
Attestation
↓
Independent Verification
↓
Policy Enforcement
↓
Admission / Rejection

Local usage

You can build the artifact locally:

chmod +x tools/build_stage240_artifact.sh
./tools/build_stage240_artifact.sh

Then verify it with the external verification tool:

python3 tools/verify_external_policy.py stage240-source-bundle.tar.gz
GitHub Actions

The reusable workflow is:

slsa-governed-build

The main workflow is:

stage240-external-verification

A successful run means:

the source bundle was built
the SBOM was generated
provenance attestation was created
SBOM attestation was created
the external verification CLI validated the artifact
policy checks passed
What changed from Stage239
Stage239
CI verified attestations
CI enforced accept/reject decisions
Stage240
the same policy logic is exposed as an external verification gate
third parties can run the same verification independently
policy enforcement is no longer CI-only

So Stage240 is not just “more verification”.

It is a shift from:

CI-only admission control

to:

independently reproducible policy enforcement
External review value

For an external reviewer, Stage240 shows that the repository now supports:

attestation production
attestation verification
CI policy enforcement
external policy reproduction
independent pass/fail judgment

This is stronger than a repository that only says its own CI passed.
It shows that verification logic can be reused outside the original workflow.

Limitations

Stage240 is a real external verification step, but it is not yet a full multi-party trust framework.

For example:

it still relies on GitHub-hosted attestation retrieval
it does not yet provide multi-organization trust roots
it does not yet implement threshold approval
it does not yet include non-GitHub transparency infrastructure

Still, it is a meaningful advancement from CI-bound verification to externally enforceable verification.

Conclusion

Stage240 establishes:

policy definition
external verification CLI
external verification evidence
independent pass/fail judgment outside CI

This stage marks the transition from:

"CI verifies the build"

to:

"verification can be enforced independently outside CI"

That is the core achievement of Stage240.

License

MIT License © 2025 Motohiro Suzuki

See LICENSE
 for details.