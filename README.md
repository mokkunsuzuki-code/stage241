cat > README.md << 'EOF'
# QSP Stage236 — Signed Release / Supply Chain Security

## Overview

Stage236 introduces signed tags and signed release artifacts for QSP.

This stage extends the project from repository integrity toward distribution integrity.

The goal is to make distributed artifacts independently verifiable through:

- signed Git tags
- signed GitHub Releases
- SHA256 digest verification
- GPG signature verification

This moves QSP into the supply chain security layer.

---

## What This Stage Adds

- Signed Git tag: `v0.236`
- GitHub Release with attached artifacts
- SHA256 checksum for release artifact
- Detached GPG signature for release artifact

Release artifacts:

- artifacts/qsp-stage236.tar.gz
- artifacts/qsp-stage236.sha256
- artifacts/qsp-stage236.tar.gz.asc

---

## Why It Matters

Previous stages established:

- claim-to-evidence linkage
- signed evidence bundles
- transparency logs
- Merkle proofs
- signed checkpoints
- multi-signer verification
- GitHub verified commits

Stage236 adds authenticity guarantees for the distributed package itself.

This means:

- users can verify what they downloaded
- tampering can be detected independently
- origin can be checked cryptographically
- release integrity is no longer based only on trust in hosting

---

## Verification

### 1. Verify SHA256

shasum -a 256 artifacts/qsp-stage236.tar.gz

Compare with:

cat artifacts/qsp-stage236.sha256

---

### 2. Verify GPG Signature

gpg --verify artifacts/qsp-stage236.tar.gz.asc artifacts/qsp-stage236.tar.gz

Expected:

- Good signature from project key
- Artifact integrity confirmed

---

### 3. Verify Signed Tag

git tag -v v0.236

---

## Security Value

Stage236 improves supply chain trust:

- protects release authenticity
- enables independent verification
- strengthens artifact integrity
- bridges repository trust → distribution trust

This stage answers:

"Is the distributed artifact truly produced by the project owner?"

---

## Evolution Path

Claim  
→ Evidence  
→ CI linkage  
→ Signed evidence  
→ Transparency log  
→ Merkle proof  
→ Signed checkpoint  
→ Checkpoint history  
→ External monitor  
→ Independent verification  
→ Executable PoC  
→ Fail evidence persistence  
→ Signed fail evidence  
→ Signed bundles  
→ Multi-signer  
→ External signatures  
→ GitHub verified commits  
→ **Signed Release (Stage236)**

---

## Release

https://github.com/mokkunsuzuki-code/stage236/releases/tag/v0.236

---

## License

MIT License © 2025 Motohiro Suzuki
EOF