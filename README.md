# SBOM Attestation Test

Test for [SBOM attestation](https://github.com/actions/attest#sbom-attestation).

The OCI container for this repository has an attached OCI artifact
with an [SBOM attestation](https://github.com/actions/attest#sbom-attestation).
As its payload, the attestation embeds an SBOM in CycloneDX 1.7 format
that claims a dependency on a library known for a high-risk vulnerability.

While the test container is not actually vulnerable (it just contains this README file),
tools such as [grype](https://github.com/anchore/grype/) should detect and report
a high-risk vulnerability when scanning this container. As of July 2026,
no vulnerability gets reported because `grype` does not find the SBOM.

To reproduce:

```sh
brew install grype
grype ghcr.io/brawer-sbom-attestation-test:latest
```



