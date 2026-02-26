SCP Traffic Generator

This repository runs GitHub Actions workflows on `windows-latest` to:
- Download required SCP installer/policy/certificate files.
- Install and configure the SCP client.
- Confirm SCP connection status from the Windows registry.
- Generate controlled web traffic by opening random sites from `top-sites.json` in a detected browser (`chrome.exe` preferred, `msedge.exe` fallback).

## Workflows

- `.github/workflows/main.yml`
  - Name: `SCP Traffic Generation - McAfee - OldGen`
  - Policy: `McAfeeTenant/scp-policy-mcafee-oldgen.opg`
  - CA: `McAfeeTenant/CA_MFE_Tenant.pem`

- `.github/workflows/nextgen-main.yml`
  - Name: `SCP Traffic Generation - McAfee - NextGen`
  - Policy: `McAfeeTenant/scp-policy-mcafee-nextgen-runner-326.opg`
  - CA: `McAfeeTenant/CA_MFE_Tenant.pem`

- `.github/workflows/main-skyhigh.yml`
  - Name: `SCP Traffic Generation - Skyhigh - OldGen`
  - Policy: `SkyhighTenant/scp-policy-skyhigh-oldgen-policy-158.opg`
  - CA: `SkyhighTenant/CA_Skyhigh.pem`

- `.github/workflows/nextgen-skyhigh.yml`
  - Name: `SCP Traffic Generation - Skyhigh - NextGen`
  - Policy: `SkyhighTenant/scp-policy-skyhigh-nextgen-policy-157.opg`
  - CA: `SkyhighTenant/CA_Skyhigh.pem`

- `.github/workflows/test-with-SCP-check.yml`
  - Trigger: manual dispatch only.
  - Uses McAfee OldGen policy/certificate for ad-hoc SCP connectivity validation.

All workflows run traffic generation only when SCP connection status is `Connected`.

## Required repository secrets

- `NewSCPToken`: token that can read raw files from `NinoTolic/SCPTesterFiles`.

## Data file

- `top-sites.json`: JSON array of candidate URLs.
- The workflow randomly selects 30 URLs per run.

## Troubleshooting output

- All workflows write SCP diagnostics to `diagnostics/scp-debug.log`.
- Logs include:
  - Downloaded file metadata and SHA256 hashes.
  - Pre/post install service snapshots for SCP/Skyhigh.
  - SCP registry connection status check details.
  - Browser detection and selected browser path/version.
  - Per-URL launch status with success/failure totals.
- Diagnostics are uploaded as workflow artifacts on every run.
- A short run outcome is also written to GitHub Step Summary.
