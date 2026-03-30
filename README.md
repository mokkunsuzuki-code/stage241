Stage237 introduces **Software Bill of Materials (SBOM)** and **Build Provenance** into QSP.

This stage extends the project from:

👉 verifying *artifacts*  
to  
👉 verifying the *entire build process*

The goal is:

> **Make software not only verifiable, but traceable.**

---

## What is added in Stage237

### 1. SBOM (Software Bill of Materials)


out/sbom/stage237_repo.spdx.json


- SPDX JSON format
- Full visibility of project contents
- Enables dependency transparency and auditing

---

### 2. Reproducible Build Artifact


dist/stage237_source_bundle.tar.gz
dist/stage237_source_bundle.tar.gz.sha256


- Deterministic source bundle
- SHA256 integrity verification

---

### 3. Build Provenance

Generated via GitHub Actions:

- Source commit
- Workflow execution
- Build environment

👉 The origin of the artifact becomes verifiable

---

### 4. Artifact Attestation

Using:


actions/attest@v4


- Cryptographic attestation tied to GitHub Actions
- Tamper detection
- Verifiable build origin

---

## Workflow


.github/workflows/stage237-sbom-provenance.yml


Pipeline:

1. Build artifact
2. Generate SBOM (Syft)
3. Upload artifacts
4. Generate provenance attestation

---

## Verification

### Verify Artifact Integrity

```bash
shasum -a 256 dist/stage237_source_bundle.tar.gz
cat dist/stage237_source_bundle.tar.gz.sha256
Inspect SBOM
cat out/sbom/stage237_repo.spdx.json
Verify Provenance
gh attestation verify dist/stage237_source_bundle.tar.gz \
  -R mokkunsuzuki-code/stage237
Why this matters

Previous stages established:

Claim
→ Evidence
→ Signature
→ Transparency
→ Signed Release

Stage237 adds:

→ SBOM
→ Build Provenance
Security Impact

Stage237 ensures:

What is included (SBOM)
Where it came from (Provenance)
That it was not tampered with (Attestation)

👉 This enables Supply Chain Security

Evolution Path
Claim
→ Evidence
→ CI Integration
→ Signed Evidence
→ Transparency Log
→ Merkle Proof
→ Multi-Signature
→ Verified Commits
→ Signed Release (Stage236)
→ SBOM + Provenance (Stage237)
Conclusion

Stage237 transitions QSP into a new class of systems:

From verifiable artifacts
to verifiable software supply chains

License

MIT License © 2025 Motohiro Suzuki