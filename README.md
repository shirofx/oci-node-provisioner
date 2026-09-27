# OCI Always Free Node Provisioner

Automated Python + GitHub Actions engine to provision an **Oracle Cloud Infrastructure (OCI) Always Free Ampere A1 Compute Instance** (`VM.Standard.A1.Flex`).

## Configuration

- **Shape:** `VM.Standard.A1.Flex`
- **OCPUs:** 2
- **Memory:** 12 GB
- **Boot Volume:** 50 GB
- **OS:** Ubuntu 24.04 LTS (aarch64)
- **Region:** `eu-marseille-1`
- **Cron:** Every 20 minutes

## Required GitHub Secrets

Configure these in **Settings > Secrets and variables > Actions**:

| Secret | Value |
|---|---|
| `OCI_USER_ID` | `ocid1.user.oc1..aaaaaaaadsryeuk42yttsbm6caxyqwr7rquwejjucxrottq5b4pgn2i7foba` |
| `OCI_PRIVATE_KEY` | Content of your OCI API private key (PEM format) |
| `OCI_FINGERPRINT` | `96:67:e2:65:5e:ec:55:3a:ab:fa:a1:5a:8c:3e:cf:f7` |
| `OCI_TENANCY_ID` | `ocid1.tenancy.oc1..aaaaaaaa4vvwe2dpvta7efliu2apxsiosodkks7eofnsh3mc6dfbrshvvf3q` |
| `OCI_REGION` | `eu-marseille-1` |
| `OCI_SUBNET_ID` | `ocid1.subnet.oc1.eu-marseille-1.aaaaaaaa6kmmye4uju66a7himfvb3bcowd77vxr4mnbrvuujzmb73dg6tnnq` |
| `OCI_IMAGE_ID` | `ocid1.image.oc1.eu-marseille-1.aaaaaaaau5dnrer7nxop4xr4jhur4zkjkolqgkaqfe7li3zbfb35clmyntva` |
| `OCI_PUBLIC_SSH_KEY` | `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFEjQN6JWYJtXMkdKadhmUDHB9KRuxxS2uq1ZhD3HGKQ kglw-shiro` |

## How It Works

1. GitHub Actions triggers `bot.py` every 20 minutes (or manually via `workflow_dispatch`)
2. The script attempts to launch an instance across 3 Availability Domains (Marseille AD-1, AD-2, AD-3)
3. Retries up to 60 times with 60-second intervals
4. On success, the instance is created with your SSH key injected

## Setup Steps

1. Fork this repository
2. Add all secrets to your fork's GitHub repository (Settings > Secrets and variables > Actions)
3. Enable the workflow (Actions tab > Enable workflow)
