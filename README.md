# AlbumAir build workflows

This public repository contains secret-free reusable GitHub Actions workflows
for customer applications operated on the AlbumAir platform. Customer
repositories call an immutable commit SHA; mutable branches and tags are not a
deployment trust boundary.

The current workflow checks out the caller repository, tests its Node.js
application, publishes an image whose GHCR owner and package match the caller
repository, scans it for fixed high and critical vulnerabilities, and signs the
immutable digest through GitHub OIDC. It uploads sanitized claims and signature
evidence without retaining the OIDC token.

Call it with only the required permissions:

```yaml
jobs:
  release:
    permissions:
      contents: read
      packages: write
      id-token: write
    uses: albumair/build-workflows/.github/workflows/build.yml@FULL_COMMIT_SHA
```

Review every dependency pin and workflow change. After merging an update,
record its full commit SHA, validate a cross-organisation caller, and update
admission's current/rollback allowlist before any application adopts it.
