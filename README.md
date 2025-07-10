# Azure Pipelines Agent Ansible Role

**🔀 Fork of [GitHub Actions runner-images](https://github.com/actions/runner-images) converted to Ansible**

This Ansible role is based on the official [GitHub Actions runner-images](https://github.com/actions/runner-images) repository and installs the same comprehensive set of development tools on Ubuntu 22.04 LXC containers. Instead of building VM images, this role configures existing containers with the identical software stack used by GitHub-hosted runners.

**✨ New: Uses official scripts via git submodule - always up-to-date with upstream!**

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
- Git (for submodule initialization)

## Role Installation

### From GitHub (with submodules)

```bash
# Clone with submodules
git clone --recursive https://github.com/bicisteadm/ansible-role-ci-images.git

# Or if already cloned, initialize submodules
git submodule update --init --recursive
```

### From Ansible Galaxy (if published)

```bash
ansible-galaxy install your-username.azure-pipelines-agent

# Then initialize submodules
cd ~/.ansible/roles/your-username.azure-pipelines-agent
git submodule update --init --recursive
```

## ⚠️ Important: Git Submodule

This role uses a git submodule to source scripts directly from the official [GitHub Actions runner-images](https://github.com/actions/runner-images) repository. This ensures you always have the latest, official installation scripts.

### First-time setup:

```bash
# In the role directory
git submodule update --init --recursive
```

### Updating to latest scripts:

```bash
# Update to latest runner-images scripts
git submodule update --remote runner-images

# Commit the update
git add runner-images
git commit -m "Update runner-images submodule to latest"
```

### Checking submodule status:

```bash
git submodule status
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

## 🔄 Architecture: Packer to Ansible Conversion

This role follows the **exact sequence** from the original Packer build, ensuring 100% compatibility:

| Packer Phase | Ansible Equivalent | Purpose |
|-------------|-------------------|---------|
| Directory creation | Phase 1: Create directories | Setup workspace |
| File provisioners | Phase 2-9: Copy scripts from submodule | Copy official scripts |
| Shell provisioners | Phase 10-28: Install everything | Run official installers |
| Reboot | Phase 22: LXC compatibility | Fake VM environment |
| Cleanup & validation | Phase 23-30: Cleanup & reports | Final steps |

### 🐳 LXC Container Compatibility

Since LXC containers don't have VM-specific features, this role includes sophisticated "VM faking":

- **DMI information** - Simulates Azure VM hardware info
- **Hypervisor detection** - Creates `/sys/hypervisor/type`
- **Azure agent state** - Fake waagent and Azure metadata
- **Machine ID** - Proper systemd machine identification

This ensures all Azure-specific scripts run without modification.

## 📁 Project Structure

```
.
├── runner-images/              # Git submodule (official scripts)
│   └── images/ubuntu/
│       ├── scripts/
│       │   ├── build/         # Installation scripts
│       │   ├── helpers/       # Helper functions
│       │   ├── tests/         # Test scripts
│       │   └── docs-gen/      # Documentation scripts
│       ├── assets/
│       │   ├── post-gen/      # Post-generation scripts
│       │   └── ubuntu2204.conf
│       └── toolsets/
│           └── toolset-2204.json
├── tasks/
│   ├── main.yml              # Main installation sequence
│   ├── lxc-compatibility.yml # LXC VM faking
│   └── lxc-cleanup.yml       # Cleanup fake files
└── defaults/main.yml         # Default variables
```

## 🚀 Benefits of This Approach

1. **Always Current** - Scripts are always the latest from GitHub
2. **Official Support** - Uses unmodified official installation scripts
3. **LXC Optimized** - Works perfectly in containers
4. **Fully Compatible** - Same results as GitHub-hosted runners
5. **Maintainable** - Auto-updates when upstream changes

## 🛠️ Maintenance

### Update to Latest Runner Images

```bash
# Update submodule to latest
git submodule update --remote runner-images

# Test the updated scripts
ansible-playbook test-playbook.yml

# Commit if everything works
git add runner-images
git commit -m "Update to latest runner-images"
```

### Troubleshooting

If installation fails:

1. **Check submodule**: `git submodule status`
2. **Update submodule**: `git submodule update --init --recursive`
3. **Verify scripts exist**: `ls runner-images/images/ubuntu/scripts/build/`
4. **Check LXC compatibility**: Review `/var/log/ansible.log`

## 📄 License

Same as original runner-images: MIT License 