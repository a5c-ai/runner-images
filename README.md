# runner-images

This is an ongoing effort to provide a set of Docker images for GitHub Actions self-hosted runners.

It's maintained and driven by the community so that
we can try to go beyond the bandwidth of GitHub Actions team. The Actions team focus more on the core ARC and Actions. We, the community, focus more on our own use-cases and runner images needed for those.

This project was inspired by and started as a fork of [quipper/actions-runner](https://github.com/quipper/actions-runner). Much appreciation to the folks Quipper for sharing their awesome work!

## Features

- We fork and occasionally rebase onto [actions/runner official Dockerfile](https://github.com/actions/runner/blob/main/images/Dockerfile) for both compatibility and more features.
- We DON'T provide `latest` only because we'd love to NOT break your production environments when we introduce backward-incompatible changes to the images.
- Single `Dockerfile` for multiple use-cases and runner modes.
- Automated CI/CD pipeline for building and publishing multi-architecture images
- Support for custom image variants with different Node.js and Ruby versions

## Available Images

Images are automatically built and published to GitHub Container Registry (ghcr.io):

```bash
# Pull the latest image
docker pull ghcr.io/a5c-ai/runner-images:latest

# Pull a specific version
docker pull ghcr.io/a5c-ai/runner-images:v1.0.0

# Pull a custom variant
docker pull ghcr.io/a5c-ai/runner-images:node18.18.2-ruby3.2.2
```

All images support both `linux/amd64` and `linux/arm64` architectures.

## Building Images

Images are automatically built and published through GitHub Actions workflows:

- **Automated builds**: Push to `main` branch or create a version tag (e.g., `v1.0.0`)
- **Custom variants**: Trigger the "Build Image Variants" workflow manually to build with specific Node.js and Ruby versions

See [.github/workflows/README.md](.github/workflows/README.md) for detailed documentation.

## Usage

- You can specify "RUN_DOCKERD=true" to start a dind daemon within the runner container.
