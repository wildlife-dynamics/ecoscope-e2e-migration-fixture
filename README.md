# ecoscope-e2e-migration-fixture

Test-only workflow for the Ecoscope end-to-end tests of template version updates
(minor auto-migration, update required, retired versions). See
wildlife-dynamics/compose#3790. Based on atmorling/test-dashboard-workflow v1.0.0.

It reads events from an EarthRanger data source and builds one map plus CSV files.
Each release tag changes the configuration form on purpose:

| Tag | Change | What the E2E expects |
|---|---|---|
| v1.0.0 | baseline (= test-dashboard-workflow v1.0.0) | — |
| v1.1.0 | compatible: adds an optional **Map Title** with a default | settings carry over; workflows move to v1.1.0 on run/save |
| v1.2.0 | incompatible: **Map Title** limited to "Events Map Ungrouped" or "Events Map" | a workflow saved with another title stays on its version; saving opens the migration draft |
| v2.0.0 | major | workflows on 1.x need an update |

Approve releases only on the **custom** allowlist (compose `scripts/security/custom/workflows.yaml`),
never in the catalog.

## Build

    pixi run compile      # regenerates ecoscope-workflows-test-dashboard-workflow/ from spec.yaml

Bump `VERSION.yaml` in that folder in the same PR; merging the PR into `main` tags and
releases it (`.github/workflows/tag.yml`). Then attach the release assets with compose's
`scripts/custom-workflows/templates.py prepare`.

## License

BSD-3-Clause
