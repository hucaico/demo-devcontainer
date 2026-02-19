# Antigravity Devcontainer Template

This repository serves as a robust template for creating Antigravity-compatible Devcontainers, specifically designed to resolve common connectivity and installation issues found in recent versions (e.g., 1.18.x).

## The Problem

Users often encounter the following issues when trying to set up a Devcontainer with Antigravity:

1.  **Configuration Errors**: `devcontainer.json` complaining about missing properties like `dockerComposeFile`.
2.  **Stalled Containers**: The container starts but stalls during initialization due to incorrect volume mounting (mounting the parent directory instead of the project root), leading to massive indexing overhead.
3.  **Server Installation Failures**: The IDE fails to connect because the remote server binary isn't installed automatically. This is often caused by:
    *   Missing dependencies (like `wget`) in the Docker image.
    *   Automatic installation scripts failing silently or attempting to download from incorrect URLs.

## The Solution

This template implements a multi-layered fix to ensure a smooth "Reopen in Container" experience:

### 1. Corrected Configuration
*   **`devcontainer.json`**: Explicitly links to the `docker-compose.yml` file.
*   **`docker-compose.yml`**:
    *   **Volume Mount**: Corrects the mount to `.:/workspace` (current directory) to prevent indexing unrelated files.
    *   **Startup Command**: Overrides the default command to execute a custom server installation script before sleeping.

### 2. Dependency Management
*   **`Dockerfile`**: Includes `wget` and other essential utilities to ensure the server installation script can download the necessary binaries.

### 3. Manual Server Installation
*   **`server-install.sh`**: A robust script that:
    *   Detects if the Antigravity server is missing.
    *   Downloads the specific, compatible version of the server (e.g., 1.18.3) from a verified URL.
    *   Installs it to the correct location (`/root/.antigravity-server`).
    *   Ensures correct permissions.

## How to Use

1.  **Clone this repository** (or use it as a template).
2.  **Open in Antigravity**.
3.  **Reopen in Container**: The setup will automatically:
    *   Build the Docker image.
    *   Start the container.
    *   Run `server-install.sh` to setup the remote server.
    *   Connect the IDE successfully.

## Files of Interest

*   `.devcontainer/devcontainer.json`: Main entry point configuration.
*   `docker-compose.yml`: Orchestrates the container and mounts the install script.
*   `Dockerfile`: Defines the OS and dependencies.
*   `server-install.sh`: The magic script that fixes the server installation.
