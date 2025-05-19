---
title: "GitHub Setup" 
chapter: false
weight: 40 
---

## Setting Up GitHub for Pipeline Scanning

Before we can start scanning repositories for policy violations, we need to set up GitHub CLI and fork the example repository. This will allow us to work with the repository locally and test the pipeline scanning capabilities.

### Installing GitHub CLI

First, let's install the GitHub CLI which we'll use to interact with GitHub repositories:

```bash
#!/bin/bash
set -e
echo "Installing GitHub CLI using direct binary download..."

# Direct binary installation for GitHub CLI
GH_VERSION="2.72.0"
ARCH=$(uname -m)

echo "Architecture detected: $ARCH"

# Download the appropriate binary based on architecture
if [ "$ARCH" = "x86_64" ]; then
    echo "Downloading GitHub CLI for x86_64..."
    curl -sSL -o /tmp/gh.tar.gz "https://github.com/cli/cli/releases/download/v${GH_VERSION}/gh_${GH_VERSION}_linux_amd64.tar.gz"
elif [ "$ARCH" = "aarch64" ]; then
    echo "Downloading GitHub CLI for arm64..."
    curl -sSL -o /tmp/gh.tar.gz "https://github.com/cli/cli/releases/download/v${GH_VERSION}/gh_${GH_VERSION}_linux_arm64.tar.gz"
else
    echo "Unsupported architecture: $ARCH"
    exit 1
fi

# Extract and install
echo "Extracting GitHub CLI..."
mkdir -p /tmp/gh
tar -xzf /tmp/gh.tar.gz -C /tmp/gh --strip-components=1

echo "Installing GitHub CLI..."
sudo cp -r /tmp/gh/bin/gh /usr/local/bin/
sudo mkdir -p /usr/local/share/man/man1/
sudo cp -r /tmp/gh/share/man/man1/* /usr/local/share/man/man1/

# Clean up
rm -rf /tmp/gh /tmp/gh.tar.gz
echo "GitHub CLI installation completed"
gh --version || echo "GitHub CLI installation verification failed"
```

You can save this as a script (e.g., `install_gh.sh`) and run it with:

```bash
chmod +x install_gh.sh
./install_gh.sh
```

### Forking and Cloning the Repository

Now that you have GitHub CLI installed, let's fork and clone the example repository in one step:

1. **Authenticate with GitHub CLI**:
   ```bash
   gh auth login
   ```
   Follow the prompts to complete the authentication process.

2. **Fork and Clone the Repository**:
   ```bash
   gh repo fork nsagark/nctl-shift-left --clone=true
   cd nctl-shift-left
   ```

3. **Verify the Repository Structure**:
   ```bash
   ls -la
   ```
   
   You should see the repository contents, including:
   - Kubernetes manifests with various configurations
   - GitHub Actions workflow files in `.github/workflows/`
   - Example policies for testing

This repository will serve as your testing ground for implementing pipeline scanning with Nirmata Control Hub.

### Repository Structure Overview

The `nctl-shift-left` repository contains several important components:

1. **Kubernetes Manifests**: Sample Kubernetes configuration files that we'll scan for policy violations
2. **GitHub Actions Workflow**: Located in `.github/workflows/scan-outputs.yaml`, this workflow demonstrates how to integrate `nctl scan` into your CI/CD pipeline
3. **Example Policies**: Sample policies that will be used to validate the configurations

In the next section, we'll explore how to scan these repositories for policy violations and view the results in Nirmata Control Hub.
