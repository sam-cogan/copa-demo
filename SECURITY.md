# Security Policy

## Vulnerability Disclosure

This is a demonstration project for Copa container patching. The vulnerabilities in the `python:3.4-alpine` base image are intentional for educational purposes.

## Demo Security Process

1. **Intentional Vulnerabilities**: This project uses `python:3.4-alpine` which contains known CVEs
2. **Automated Patching**: GitHub Actions workflow automatically patches these vulnerabilities using Copa
3. **Verification**: Scan results show before/after vulnerability comparison

## Real-World Usage

For production use of Copa:

- Use current, supported base images when possible
- Implement Copa as part of your security pipeline
- Regularly update Copa and scanning tools
- Monitor security advisories for your dependencies

## Reporting Issues

For issues with this demo project, please open a GitHub issue.

For Copa-related security concerns, report to the [Copa project](https://github.com/project-copacetic/copacetic/security).
