SCP Traffic Generator

This repository runs a GitHub Actions workflow on `windows-latest` to:
- Download required SCP installer/policy/certificate files.
- Install and configure the SCP client.
- Confirm SCP connection status from the Windows registry.
- Generate controlled web traffic by opening random sites from `top-sites.json` in headless Chrome.

## Workflows

- `.github/workflows/main.yml`
  - Trigger: daily schedule (`0 2 * * *`) and manual dispatch.
  - Uses Chrome headless launch with explicit window-size and flags.
  - Traffic generation runs only when SCP connection status is `Connected`.

- `.github/workflows/test-with-SCP-check.yml`
  - Trigger: manual dispatch only.
  - Same SCP-check-gated traffic generation behavior for ad-hoc validation.

## Required repository secrets

- `NewSCPToken`: token that can read raw files from `NinoTolic/SCPTesterFiles`.

## Data file

- `top-sites.json`: JSON array of candidate URLs.
- The workflow randomly selects 30 URLs per run.

## Troubleshooting output

- Both workflows now write SCP diagnostics to `diagnostics/scp-debug.log`.
- Logs include:
  - Downloaded file metadata and SHA256 hashes.
  - Pre/post install service snapshots for SCP/Skyhigh.
  - SCP registry connection status check details.
  - Chrome path/version check and selected URL count.
- Diagnostics are uploaded as workflow artifacts on every run.
- A short run outcome is also written to GitHub Step Summary.
