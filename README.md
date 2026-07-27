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

## To reproduce

```sh
$ brew install grype
$ grype ghcr.io/brawer/sbom-attestation-test:latest
```

### Expected

```
$ grype ghcr.io/brawer/sbom-attestation-test:latest
 ✔ Scanned for vulnerabilities     [0 vulnerability matches]  
   ├── by severity: 2 critical, 1 high, 4 medium, 0 low, 0 negligible
   └── by status:   7 fixed, 0 not-fixed, 0 ignored

NAME        INSTALLED  FIXED IN  TYPE          VULNERABILITY        SEVERITY  EPSS            RISK          
log4j-core  2.14.1     2.15.0    java-archive  GHSA-jfh8-c2jp-5v3q  Critical  100.0% (100th)  100.0   KEV   
log4j-core  2.14.1     2.16.0    java-archive  GHSA-7rjr-3q55-vv33  Critical  100.0% (99th)   99.0    KEV   
log4j-core  2.14.1     2.17.0    java-archive  GHSA-p6xc-xr62-6r2g  High      100.0% (99th)   80.5          
log4j-core  2.14.1     2.17.1    java-archive  GHSA-8489-44mv-ggj8  Medium    97.9% (99th)    56.8          
log4j-core  2.14.1     2.25.4    java-archive  GHSA-3pxv-7cmr-fjr4  Medium    0.9% (54th)     0.5           
log4j-core  2.14.1     2.25.3    java-archive  GHSA-vc5p-v9hr-52mj  Medium    0.8% (51st)     0.4           
log4j-core  2.14.1     2.25.4    java-archive  GHSA-6hg6-v5c8-fphq  Medium    0.4% (33rd)     0.2
```

### Observed

```
$ grype ghcr.io/brawer/sbom-attestation-test:latest
 ✔ Parsed image                                sha256:8e479487a51ed4ac3e390927c8f3ff40b76785f551bea521184f2e64e218c10a
 ✔ Cataloged contents                                 33205b8b57bb4b7e1bfcbf57829fe8309a1f0ef00f481a964210fcb044b75c29
   ├── ✔ Packages                        [0 packages]  
   └── ✔ Executables                     [0 executables]  
 ✔ Scanned for vulnerabilities     [0 vulnerability matches]  
   ├── by severity: 0 critical, 0 high, 0 medium, 0 low, 0 negligible
   └── by status:   0 fixed, 0 not-fixed, 0 ignored 
No vulnerabilities found
```


