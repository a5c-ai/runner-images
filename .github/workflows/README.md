# GitHub Actions Workflows

This directory contains GitHub Actions workflows for building and publishing Docker images for GitHub Actions self-hosted runners.

## Workflows

### 1. Build and Publish Docker Images (`build-and-publish.yml`)

This is the main workflow that automatically builds and publishes Docker images to GitHub Container Registry (ghcr.io).

**Triggers:**
- Push to `main` or `master` branch
- Push of tags starting with `v*` (e.g., `v1.0.0`, `v2.1.3`)
- Pull requests to `main` or `master` branch (builds but doesn't push)
- Manual workflow dispatch with optional version overrides

**Features:**
- Multi-architecture builds (linux/amd64, linux/arm64)
- Automatic tagging based on branch, PR, semantic version, and commit SHA
- Caching for faster subsequent builds
- Artifact attestation for supply chain security

**Image Tags Generated:**
- `latest` - Latest build from default branch
- `main` or `master` - Latest build from the respective branch
- `pr-<number>` - Build from pull request
- `v1.0.0`, `v1.0`, `v1` - Semantic version tags (when pushing version tags)
- `main-<sha>` - Branch name with commit SHA

**Manual Trigger:**
You can manually trigger this workflow from the Actions tab with custom versions:
- Runner version
- Runner container hooks version
- Docker version

### 2. Build Image Variants (`build-variants.yml`)

This workflow allows building custom image variants with different versions of Node.js, Ruby, and other components.

**Triggers:**
- Manual workflow dispatch only

**Features:**
- Customizable Node.js version
- Customizable Ruby version
- Customizable runner, hooks, and Docker versions
- Custom image tag support
- Multi-architecture builds (linux/amd64, linux/arm64)

**Manual Trigger:**
Run this workflow from the Actions tab to build a custom variant:
1. Go to Actions → Build Image Variants
2. Click "Run workflow"
3. Specify versions:
   - Node.js version (default: 18.18.2)
   - Ruby version (default: 3.2.2)
   - GitHub Actions Runner version (default: 2.310.2)
   - Runner Container Hooks version (default: 0.3.2)
   - Docker version (default: 23.0.6)
4. Optionally specify a custom image tag

**Default Image Tag Format:**
`node<version>-ruby<version>` (e.g., `node18.18.2-ruby3.2.2`)

## Using the Images

After the workflows complete, images are available at:
```
ghcr.io/a5c-ai/runner-images:<tag>
```

### Example: Pull the latest image
```bash
docker pull ghcr.io/a5c-ai/runner-images:latest
```

### Example: Pull a specific version
```bash
docker pull ghcr.io/a5c-ai/runner-images:v1.0.0
```

### Example: Pull a custom variant
```bash
docker pull ghcr.io/a5c-ai/runner-images:node18.18.2-ruby3.2.2
```

## Registry Permissions

Images are published to GitHub Container Registry (ghcr.io). The workflows use the `GITHUB_TOKEN` which is automatically provided by GitHub Actions. No additional secrets are required.

To pull images, you may need to authenticate:
```bash
echo $GITHUB_TOKEN | docker login ghcr.io -u USERNAME --password-stdin
```

## Build Arguments

The Dockerfile supports the following build arguments:

- `TARGETOS` - Target OS (linux)
- `TARGETARCH` - Target architecture (amd64, arm64)
- `RUNNER_VERSION` - GitHub Actions runner version (default: 2.310.2)
- `RUNNER_CONTAINER_HOOKS_VERSION` - Container hooks version (default: 0.3.2)
- `DOCKER_VERSION` - Docker version (default: 23.0.6)
- `NODE_VERSION` - Node.js version (default: 18.18.2)
- `RUBY_VERSION` - Ruby version (default: 3.2.2)

## Caching

Both workflows use GitHub Actions cache to speed up subsequent builds:
- Docker layer caching is enabled
- Cache mode is set to `max` for optimal caching

## Security

- Artifact attestations are generated for all published images
- SLSA provenance is attached to provide supply chain transparency
- Images are scanned and signed automatically
