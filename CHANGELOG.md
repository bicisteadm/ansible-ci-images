# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Planned
- Support for Ubuntu 24.04
- Configuration for different agent types (build, deployment, testing)
- Integration with Azure Key Vault for secrets
- Support for Windows containers

## [1.0.0] - 2024-01-XX

### Added
- Initial role release
- Support for Ubuntu 22.04 LXC containers
- Complete development tools suite:
  - Basic tools (PowerShell, Git, APT packages)
  - CI/CD tools (GitHub Actions, Runner Package)
  - Azure tools (CLI, DevOps CLI, AzCopy, Bicep)
  - Cloud tools (AWS CLI, Google Cloud CLI, Aliyun CLI)
  - Development tools (CMake, CodeQL, Bazel, VCPKG)
  - Compilers (GCC, Clang, Swift, GFortran)
  - Version control tools (Git, Git LFS, GitHub CLI)
  - Programming languages (Python, Node.js, Java, Ruby, Rust, Julia, R, etc.)
  - Infrastructure tools (Terraform, Packer, Kubernetes, OpenShift)
  - Utility tools (YQ, Zstd, Heroku CLI)
  - Database tools (MySQL, PostgreSQL, MSSQL Tools)
  - Browsers and testing tools (Chrome, Firefox, Edge, Selenium)
  - Container tools (Docker, container-tools)
  - Specialized tools (Android SDK, ORAS CLI)
  - Package managers (Homebrew)
- LXC compatibility with fake Azure files
- Configurable installations using boolean variables
- Complete documentation in English
- Usage examples for various scenarios
- GitHub Actions workflow for CI/CD
- Test suite
- Ansible-lint compatibility

### Security
- All scripts are copied with appropriate permissions
- Support for Docker Hub credentials
- Secure environment variable handling

### Documentation
- Complete README.md in English
- Configuration examples for different use cases
- Documentation of all variables
- Inventory file examples
- Guidance for tags and selective installation

## [0.1.0] - 2024-01-XX

### Added
- Initial development
- Conversion from original playbook format
- Basic role structure 