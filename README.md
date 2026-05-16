# moneyprinter-turbo-docker

Automated Docker image builder for [`harry0703/MoneyPrinterTurbo`](https://github.com/harry0703/MoneyPrinterTurbo).

This repo contains **only** a GitHub Actions workflow. It watches the upstream repository for new releases and publishes the resulting container images to **GitHub Container Registry (GHCR)**.

No upstream source code is mirrored here. Each build checks out the tagged commit of the upstream repository at run time and builds its `Dockerfile` / `Dockerfile.gpu`.

## Images

| Variant | Tag pattern                                          | Dockerfile       |
| ------- | ---------------------------------------------------- | ---------------- |
| CPU     | `ghcr.io/xinitteam/moneyprinter-turbo:<version>`     | `Dockerfile`     |
| CPU     | `ghcr.io/xinitteam/moneyprinter-turbo:latest`        | `Dockerfile`     |
| GPU     | `ghcr.io/xinitteam/moneyprinter-turbo:<version>-gpu` | `Dockerfile.gpu` |
| GPU     | `ghcr.io/xinitteam/moneyprinter-turbo:latest-gpu`    | `Dockerfile.gpu` |

`<version>` matches the upstream release tag (e.g. `v1.2.7`).

## How it works

- **Schedule**: a cron job runs daily at `03:17 UTC` and queries the latest upstream release.
- **Idempotency**: if an image for that tag already exists in GHCR, the build is skipped.
- **Manual dispatch**: trigger `build` via the Actions tab with an explicit `tag` input (and optional `force` to rebuild).
- **No secrets required**: authentication to GHCR uses the workflow's `GITHUB_TOKEN`.

## Usage

```bash
# CPU
docker pull ghcr.io/xinitteam/moneyprinter-turbo:latest

# GPU (requires NVIDIA Container Toolkit on host)
docker pull ghcr.io/xinitteam/moneyprinter-turbo:latest-gpu
```

Refer to the [upstream README](https://github.com/harry0703/MoneyPrinterTurbo#readme) for runtime configuration, environment variables, and usage instructions.

## License

The workflow code in this repository is released under the MIT License. The built images contain MoneyPrinterTurbo, which is licensed under the [MIT License](https://github.com/harry0703/MoneyPrinterTurbo/blob/main/LICENSE) by the upstream authors.
