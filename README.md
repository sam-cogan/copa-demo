# Copa Demo - Container Image Vulnerability Patching

This repository demonstrates how to use [Copa (Container Patching)](https://github.com/project-copacetic/copacetic) with GitHub Actions to automatically patch vulnerable container images.

## Overview

This GitHub Action workflow scans container images for OS package vulnerabilities using Trivy, and automatically patches them using Copa when vulnerabilities are found. The patched images are then pushed to GitHub Container Registry (GHCR).

## How It Works

1. **Vulnerability Scanning**: Uses [Trivy](https://github.com/aquasecurity/trivy) to scan container images for OS package vulnerabilities
2. **Vulnerability Assessment**: Checks if any fixable OS package vulnerabilities exist
3. **Patching**: If vulnerabilities are found, uses the [Copa Action](https://github.com/project-copacetic/copa-action) to patch the image
4. **Publishing**: Pushes the patched image to GitHub Container Registry

## Workflow Configuration

The workflow is configured in `.github/workflows/copa-patch.yaml` and includes:

### Trigger Events
- Push to `main` branch (excluding markdown files)
- Manual workflow dispatch

### Image Matrix
Currently configured to patch this image:
- `docker.io/library/python:3.4-alpine`

### Key Features
- **Conditional Execution**: Only runs Copa patching if vulnerabilities are detected
- **Timeout Protection**: 5-minute timeout for patching operations
- **VEX Generation**: Generates Vulnerability Exchange (VEX) documents in OpenVEX format
- **Secure Publishing**: Uses GitHub token for authentication to GHCR

## Prerequisites

### Repository Permissions
Ensure your repository has the following permissions configured:
- `contents: read` - For reading repository content
- `packages: write` - For pushing to GitHub Container Registry

### Secrets
The workflow uses the built-in `GITHUB_TOKEN` secret, which is automatically available in GitHub Actions.

## Customization

### Adding Your Own Images
Modify the `matrix.images` section in the workflow file:

```yaml
strategy:
  matrix:
    images:
      - "your-registry/your-image:tag"
      - "another-registry/another-image:tag"
```

### Changing Trigger Events
Modify the `on` section to suit your needs:

```yaml
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * *'  # Daily at 2 AM
```

### Registry Configuration
To push to a different container registry, modify the login step and push commands. See the [docker/login-action documentation](https://github.com/docker/login-action#usage) for examples.

## Output

When vulnerabilities are found and patched:
- **Patched Image**: Tagged with `-patched` suffix
- **VEX Document**: Generated as `vex.json` containing vulnerability information
- **Registry Push**: Patched image pushed to `ghcr.io/project-copacetic/copa-action/test/`

## Security Considerations

- The workflow uses pinned action versions with digests for security
- Only scans for OS package vulnerabilities (not application dependencies)
- Ignores unfixable vulnerabilities to focus on actionable issues
- Uses minimal required permissions

## Local Development

To test Copa locally:

```bash
# Install Copa
curl -sSfL https://raw.githubusercontent.com/project-copacetic/copacetic/main/install.sh | sh

# Scan an image with Trivy
trivy image --format json --output report.json alpine:3.18.4

# Patch the image with Copa
copa patch -i alpine:3.18.4 -r report.json -t alpine:3.18.4-patched
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with your own images
5. Submit a pull request

## Resources

- [Copa Documentation](https://github.com/project-copacetic/copacetic)
- [Copa GitHub Action](https://github.com/project-copacetic/copa-action)
- [Trivy Documentation](https://trivy.dev/)
- [OpenVEX Specification](https://github.com/openvex/spec)

## License

This project is provided as-is for demonstration purposes. Please refer to the individual tool licenses for Copa, Trivy, and related components.
