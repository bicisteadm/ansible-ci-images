# Azure Pipelines Agent Ansible Role

This Ansible role installs Azure Pipelines Agent on Ubuntu 22.04 LXC containers with a complete set of development tools.

## Overview

The role automatically installs:
- **Basic tools**: PowerShell, Git, compilers (GCC, Clang, Swift)
- **CI/CD tools**: GitHub Actions Cache, GitHub CLI, Azure DevOps CLI
- **Cloud tools**: Azure CLI, AWS CLI, Google Cloud CLI, Terraform, Packer
- **Programming languages**: Python, Node.js, Java, Ruby, Rust, Go, PHP, R, Julia
- **Databases**: MySQL, PostgreSQL, MSSQL tools
- **Containers**: Docker, Kubernetes tools, OpenShift CLI
- **Browsers**: Chrome, Firefox, Edge + Selenium
- **Specialized tools**: Android SDK, Miniconda, Homebrew

## Requirements

- Ubuntu 22.04 LXC container
- Ansible 2.9+
- Root access or sudo privileges
- Internet connection

## Role Installation

### From GitHub

```bash
ansible-galaxy install git+https://github.com/your-username/azure-pipelines-agent.git
```

### From Ansible Galaxy (if published)

```bash
ansible-galaxy install your-username.azure-pipelines-agent
```

## Usage

### Basic Usage

```yaml
---
- name: Install Azure Pipelines Agent
  hosts: lxc_containers
  become: true
  roles:
    - azure-pipelines-agent
```

### Advanced Usage with Custom Variables

```yaml
---
- name: Install Azure Pipelines Agent with custom configuration
  hosts: lxc_containers
  become: true
  roles:
    - role: azure-pipelines-agent
      vars:
        # Basic configuration
        image_version: "20250629.1.0"
        image_os: "ubuntu22"
        
        # Optional Docker Hub credentials
        dockerhub_login: "your-dockerhub-username"
        dockerhub_password: "your-dockerhub-password"
        
        # Individual category installation control
        install_basic_packages: true
        install_ci_cd_tools: true
        install_azure_tools: true
        install_cloud_tools: false          # Skip cloud tools
        install_development_tools: true
        install_compilers: true
        install_version_control: true
        install_programming_languages: true
        install_infrastructure_tools: false # Skip infrastructure
        install_utilities: true
        install_databases: false            # Skip databases
        install_browsers: false             # Skip browsers
        install_container_tools: true
        install_specialized_tools: false    # Skip specialized tools
        install_package_managers: true
        
        # System configuration
        configure_system_limits: true
        configure_environment: true
        configure_dpkg: true
        configure_snap: true
```

### Usage with Inventory

```ini
[lxc_containers]
container1 ansible_host=192.168.1.100 ansible_user=ubuntu
container2 ansible_host=192.168.1.101 ansible_user=ubuntu

[lxc_containers:vars]
ansible_python_interpreter=/usr/bin/python3
```

## Role Variables

### Basic Configuration

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `image_version` | Image version | `"20250629.1.0"` |
| `image_os` | Operating system | `"ubuntu22"` |
| `debian_frontend` | Debian frontend mode | `"noninteractive"` |

### Directory Structure

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `image_folder` | Main installation directory | `"/imagegeneration"` |
| `installer_script_folder` | Installation scripts directory | `"/imagegeneration/installers"` |
| `helper_script_folder` | Helper scripts directory | `"/imagegeneration/helpers"` |
| `toolcache_root` | Tools directory | `"/opt/hostedtoolcache"` |

### Optional Credentials

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `dockerhub_login` | Docker Hub username | `""` |
| `dockerhub_password` | Docker Hub password | `""` |

### Installation Control

All following variables are `boolean` type with default value `true`:

- `install_basic_packages` - Basic packages and PowerShell
- `install_ci_cd_tools` - CI/CD tools (GitHub Actions, Runner Package)
- `install_azure_tools` - Azure tools (CLI, DevOps CLI, AzCopy, Bicep)
- `install_cloud_tools` - Cloud tools (AWS, GCP, Aliyun)
- `install_development_tools` - Development tools (CMake, CodeQL, Bazel)
- `install_compilers` - Compilers (GCC, Clang, Swift, GFortran)
- `install_version_control` - Version control tools (Git, Git LFS, GitHub CLI)
- `install_programming_languages` - Programming languages (Python, Node.js, Java, Ruby, etc.)
- `install_infrastructure_tools` - Infrastructure tools (Terraform, Packer, Kubernetes)
- `install_utilities` - Utility tools (YQ, Zstd, Heroku CLI)
- `install_databases` - Database tools (MySQL, PostgreSQL, MSSQL)
- `install_browsers` - Browsers and testing tools (Chrome, Firefox, Selenium)
- `install_container_tools` - Container tools (Docker, container-tools)
- `install_specialized_tools` - Specialized tools (Android SDK, ORAS CLI)
- `install_package_managers` - Package managers (Homebrew)

### System Configuration

- `configure_system_limits` - System limits configuration
- `configure_environment` - Environment configuration
- `configure_dpkg` - DPKG configuration
- `configure_snap` - Snap configuration

## Examples of Different Configurations

### Minimal Installation (basic tools only)

```yaml
- role: azure-pipelines-agent
  vars:
    install_ci_cd_tools: false
    install_azure_tools: false
    install_cloud_tools: false
    install_development_tools: false
    install_compilers: false
    install_programming_languages: false
    install_infrastructure_tools: false
    install_utilities: false
    install_databases: false
    install_browsers: false
    install_container_tools: false
    install_specialized_tools: false
    install_package_managers: false
```

### Web Development Only

```yaml
- role: azure-pipelines-agent
  vars:
    install_programming_languages: true  # Node.js, Python, PHP, Ruby
    install_databases: true              # MySQL, PostgreSQL
    install_browsers: true               # Chrome, Firefox, Selenium
    install_container_tools: true        # Docker
    install_version_control: true        # Git, GitHub CLI
    
    # Skip others
    install_azure_tools: false
    install_cloud_tools: false
    install_compilers: false
    install_infrastructure_tools: false
    install_specialized_tools: false
```

### DevOps Only

```yaml
- role: azure-pipelines-agent
  vars:
    install_ci_cd_tools: true
    install_azure_tools: true
    install_cloud_tools: true
    install_infrastructure_tools: true
    install_container_tools: true
    install_version_control: true
    
    # Skip development tools
    install_programming_languages: false
    install_compilers: false
    install_databases: false
    install_browsers: false
    install_specialized_tools: false
```

## Tags

The role supports the following tags for selective execution:

- `always` - Basic configuration (always runs)
- `ci-cd` - CI/CD tools
- `azure` - Azure tools
- `cloud` - Cloud tools
- `development` - Development tools
- `compilers` - Compilers
- `version-control` - Version control
- `programming-languages` - Programming languages
- `infrastructure` - Infrastructure tools
- `utilities` - Utility tools
- `databases` - Databases
- `browsers` - Browsers
- `containers` - Containers
- `specialized` - Specialized tools
- `package-managers` - Package managers

### Example Usage with Tags

```bash
# Install only Azure tools
ansible-playbook -i inventory playbook.yml --tags "azure"

# Install only programming languages and databases
ansible-playbook -i inventory playbook.yml --tags "programming-languages,databases"

# Skip browsers
ansible-playbook -i inventory playbook.yml --skip-tags "browsers"
``` 