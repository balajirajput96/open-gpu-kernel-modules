# Deployment Documentation

## Overview

This repository includes automated CI/CD workflows using GitHub Actions to build and deploy the NVIDIA Open GPU Kernel Modules.

## Workflows

### 1. CI Workflow (`ci.yml`)

**Triggers:**
- All pull requests
- Pushes to `main` or `master` branches

**Purpose:**
- Quick validation of code changes
- Builds kernel modules to ensure compilation succeeds
- Verifies that build artifacts are created

**Steps:**
1. Checkout code
2. Install build dependencies (gcc, make, linux-headers)
3. Build kernel modules using `make modules`
4. Verify that `.ko` files were created

### 2. Build and Deploy Workflow (`build-and-deploy.yml`)

**Triggers:**
- Pushes to `main` or `master` branches
- Pull requests to `main` or `master` branches
- Manual trigger via workflow_dispatch

**Purpose:**
- Complete CI/CD pipeline
- Build kernel modules for x86_64 architecture
- Create release artifacts
- Automatically deploy releases when code is merged to main/master

**Build Job:**
1. Checkout code
2. Install dependencies
3. Build kernel modules for x86_64
4. Upload build artifacts (retained for 30 days)

**Deploy Job:**
- Only runs on pushes to main/master branches
- Downloads build artifacts
- Extracts version from `version.mk`
- Creates GitHub Release with built kernel modules
- Tags release as `release-{version}`

## Architecture Support

Currently configured for:
- x86_64 (default)

To add support for additional architectures (e.g., aarch64), modify the matrix strategy in `build-and-deploy.yml`.

## Manual Deployment

You can manually trigger the deployment workflow:
1. Go to the "Actions" tab in GitHub
2. Select "Build and Deploy" workflow
3. Click "Run workflow"
4. Choose the branch and click "Run workflow"

## Release Process

Releases are created automatically when:
1. Code is pushed to `main` or `master` branch
2. Build succeeds
3. Version is extracted from `version.mk`
4. GitHub Release is created with tag `release-{version}`
5. Kernel module artifacts (`.ko` files) are attached to the release

## Requirements

The workflows require:
- Ubuntu Linux runner
- Build tools: gcc, make, build-essential
- Linux kernel headers

## Troubleshooting

If builds fail:
1. Check the Actions tab for detailed logs
2. Verify that the code builds locally using `make modules`
3. Ensure all required dependencies are available
4. Check that kernel headers are compatible

## Future Enhancements

Potential improvements:
- Add multi-architecture builds (aarch64)
- Add code quality checks (linting, static analysis)
- Add automated tests
- Add Docker-based builds for consistency
- Add changelog generation
