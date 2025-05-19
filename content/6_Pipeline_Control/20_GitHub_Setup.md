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

### GitHub Authentication

Let's now authenticate with GitHub CLI:

1. Check if you're already authenticated with GitHub:

   ```bash
   gh auth status || gh auth login
   ```

2. If you need to authenticate, follow these steps:
   - Select **GitHub.com**
   - Select **HTTPS** as your preferred protocol
   - When asked "Authenticate Git with your GitHub credentials?": Enter **Y**
   - Select **Login with a web browser**
   - Copy the one-time code shown in your terminal
   - Press Enter to open the browser
   - Paste the code in GitHub and authorize access
   - Return to your terminal and wait for authentication to complete (This can take up to 30 seconds or more)

![GitHub CLI Authentication](/images/gh-auth.png)

::alert[If you don't have the GitHub CLI installed, you can install it with the [installation instructions](https://github.com/cli/cli#installation) for your operating system.]{header="Note"}

## Fork the Workshop Repository

For this workshop, we'll fork the Akuity EKS workshop template repository. **Choose ONE of the following methods**:

### Option 1: Using GitHub CLI (Recommended)

Fork and clone the repository using the GitHub CLI:

```bash
# Fork the repository and clone it
gh repo fork nsagark/nctl-shift-left --clone=true --remote=true
cd nctl-shift-left
```

### Option 2: Using GitHub Web Interface

If you prefer to use the GitHub web interface instead:

1. Visit [https://github.com/nirmata/demo-nctl-shift-left](https://github.com/nirmata/demo-nctl-shift-left)
2. Click the "Fork" button in the top-right corner
3. Ensure your personal account is selected as the owner
4. Keep the repository name as `nctl-shift-left`
5. Click "Create fork"
6. Clone the repository to your local machine:
   ```bash
   git clone https://github.com/YOUR-USERNAME/nctl-shift-left
   cd nctl-shift-left
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
