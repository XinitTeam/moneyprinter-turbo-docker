# moneyprinter-turbo-docker

Automated Docker image builder for [`harry0703/MoneyPrinterTurbo`](https://github.com/harry0703/MoneyPrinterTurbo).

This repo contains **only** a GitHub Actions workflow. It watches the upstream repository for new releases and publishes the resulting container images to **GitHub Container Registry (GHCR)**.

No upstream source code is mirrored here. Each build checks out the tagged commit of the upstream repository at run time and builds its `Dockerfile` / `Dockerfile.gpu`.

## Images

| Variant | Tag pattern                                                 | Source           | Dockerfile       |
| ------- | ----------------------------------------------------------- | ---------------- | ---------------- |
| CPU     | `ghcr.io/xinitteam/moneyprinter-turbo:<version>`            | upstream release | `Dockerfile`     |
| CPU     | `ghcr.io/xinitteam/moneyprinter-turbo:latest`               | upstream release | `Dockerfile`     |
| CPU     | `ghcr.io/xinitteam/moneyprinter-turbo:main-<short-sha>`     | upstream `main`  | `Dockerfile`     |
| CPU     | `ghcr.io/xinitteam/moneyprinter-turbo:main`                 | upstream `main`  | `Dockerfile`     |
| GPU     | `ghcr.io/xinitteam/moneyprinter-turbo:<version>-gpu`        | upstream release | `Dockerfile.gpu` |
| GPU     | `ghcr.io/xinitteam/moneyprinter-turbo:latest-gpu`           | upstream release | `Dockerfile.gpu` |
| GPU     | `ghcr.io/xinitteam/moneyprinter-turbo:main-<short-sha>-gpu` | upstream `main`  | `Dockerfile.gpu` |
| GPU     | `ghcr.io/xinitteam/moneyprinter-turbo:main-gpu`             | upstream `main`  | `Dockerfile.gpu` |

`<version>` matches the upstream release tag (e.g. `v1.2.7`). `<short-sha>` is the 7-character abbreviation of the upstream `main` commit.

## How it works

Two workflows run independently:

- **`build`** (release tags): daily at `03:17 UTC`, picks the latest upstream release and builds it. Manual dispatch accepts an explicit `tag` input.
- **`build-main`** (rolling main): daily at `04:17 UTC`, picks the current `HEAD` of upstream `main` and builds it. Tagged with `main-<short-sha>` for traceability plus the moving `main` tag.
- **Idempotency**: if an image for that tag (release tag or `main-<short-sha>`) already exists in GHCR, the build is skipped. Override with the `force` input.
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
