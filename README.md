# OCI Always Free Node Provisioner

Automated Python + GitHub Actions engine to provision an **Oracle Cloud Infrastructure (OCI) Always Free Ampere A1 Compute Instance** (`VM.Standard.A1.Flex`).

## Configuration

- **Shape:** `VM.Standard.A1.Flex`
- **OCPUs:** 2 (fallback to 1 if capacity unavailable)
- **Memory:** 12 GB (fallback to 6 GB)
- **Boot Volume:** 50 GB
- **OS:** Ubuntu 24.04 LTS (aarch64)
- **Region:** `us-phoenix-1` (configurable)
- **Cron:** Every 20 minutes

## Required GitHub Secrets

Configure these in **Settings > Secrets and variables > Actions**:

| Secret | Description |
|---|---|
| `OCI_USER_ID` | OCID of your OCI user (`ocid1.user.oc1..aaaa...`) |
| `OCI_PRIVATE_KEY` | Content of your OCI API private key (PEM format) |
| `OCI_FINGERPRINT` | Fingerprint of your OCI API key |
| `OCI_TENANCY_ID` | OCID of your tenancy (`ocid1.tenancy.oc1..aaaa...`) |
| `OCI_REGION` | OCI region (e.g. `us-phoenix-1`) |
| `OCI_SUBNET_ID` | OCID of your public subnet (`ocid1.subnet.oc1.phx..aaaa...`) |
| `OCI_IMAGE_ID` | OCID of Ubuntu 24.04 aarch64 image (`ocid1.image.oc1.phx..aaaa...`) |
| `OCI_PUBLIC_SSH_KEY` | Your SSH public key (`ssh-rsa AAAA...` or `ssh-ed25519 AAAA...`) |

## How It Works

1. GitHub Actions triggers `bot.py` every 20 minutes (or manually via `workflow_dispatch`)
2. The script attempts to launch an instance across 3 Availability Domains (PHX-AD-1, AD-2, AD-3)
3. First tries 2 OCPU / 12 GB, then falls back to 1 OCPU / 6 GB if capacity is unavailable
4. Retries up to 60 times with 60-second intervals
5. On success, the instance is created with your SSH key injected

## Setup Steps

1. Fork this repository
2. Create an OCI API key: Console > User Settings > API Keys > Add API Key
3. Download the `.pem` file and note the fingerprint
4. Create a VCN with a public subnet (use the VCN Wizard)
5. Get the Ubuntu 24.04 aarch64 image OCID from the OCI Console
6. Add all secrets to your fork's GitHub repository
7. Enable the workflow (Actions tab > Enable workflow)
