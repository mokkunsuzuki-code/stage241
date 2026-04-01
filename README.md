# Stage241: Multi-Party Verification Gate

MIT License © 2025 Motohiro Suzuki

## Overview

Stage241 upgrades verification from external validation to multi-party approval.

Stage240 made it possible to verify artifacts outside CI.

Stage241 goes further.

This stage introduces threshold-based verification, where multiple independent signer identities are required for approval.

A build is accepted only if the required number of signatures is satisfied.

Current configuration:

- owner signer
- auditor signer
- threshold: 2-of-2

This means a single signer is no longer sufficient.

---

## Why this stage matters

This stage upgrades the trust model.

### Before (Stage240)
- external verification possible
- policy enforced outside CI

### After (Stage241)
- multiple signer identities required
- approval distributed across keys
- single-key compromise is insufficient

This is a shift from verification to distributed approval.

---

## Core concept

Stage241 introduces:

- multiple signer identities
- threshold policy
- signature verification per signer
- pass/fail based on signer count

The decision is no longer binary from a single authority.

---

## Repository structure


keys/
owner_public.pem
auditor_public.pem

policy/
policy.yaml

signatures/
owner.sig
auditor.sig

tools/
build_stage241_artifact.sh
sign_stage241_artifact.sh
verify_multi_party.py

docs/
multi_party_verification.md

out/
multi_party/


---

## Workflow

The Stage241 process:

Build artifact  
↓  
Sign with owner key  
↓  
Sign with auditor key  
↓  
Verify each signature  
↓  
Check threshold (2-of-2)  
↓  
Pass / Fail  

---

## Local usage

Build artifact:

```bash
./tools/build_stage241_artifact.sh

Sign artifact:

./tools/sign_stage241_artifact.sh

Verify multi-party threshold:

python3 tools/verify_multi_party.py stage241-source-bundle.tar.gz
CI behavior

GitHub Actions in Stage241:

validates repository structure
ensures policy and public keys exist
verifies the threshold verification tool integrity

Important:

Private signing keys are intentionally excluded from CI.

This ensures:

no secret leakage
signing remains external
verification remains reproducible
What changed from Stage240
Stage240
verification can be performed externally
CI-independent validation
Stage241
multiple approvals required
threshold-based decision
single signer no longer sufficient

This is a transition from:

"anyone can verify"

to:

"multiple parties must approve"

Security meaning

Stage241 demonstrates:

separation of signer identities
removal of single-key authority
threshold-based acceptance
stronger resistance to key compromise

Even if one key is compromised, approval is not granted.

Limitations

Current implementation:

both keys are controlled by the same operator context
not yet a fully independent third-party trust model

However:

identity separation is established
threshold enforcement is implemented
architecture supports external expansion
Future direction

Next evolution (Stage242):

introduce external reviewer key
move to 2-of-3 or 3-of-3 threshold
enable true distributed trust
Conclusion

Stage241 establishes:

multi-party verification
threshold enforcement
distributed approval model
CI-safe architecture

This stage marks the transition from:

"verification is reproducible"

to:

"approval requires multiple independent identities"

License

MIT License © 2025 Motohiro Suzuki

See LICENSE
 for details.
