# Server Setup Log

## Initial Setup
The server was initially set up by installing **Ubuntu 22.04**.

## Requirements
The primary requirements for the server were to support:
- OpenMPI
- OpenMP
- CUDA

## Solution
To meet these requirements, a **JupyterHub** instance was set up. 
- We utilized **DockerSpawner** to manage user environments.
- The setup includes full support for **GPU** acceleration, ensuring users can leverage CUDA for deep learning tasks.

### Build Process
1.  **Download Driver**: The `nvhpc_2025_253_Linux_x86_64_cuda_12.8.tar.gz` driver was downloaded first.
2.  **Build Image**: The Docker image was then built, incorporating the downloaded driver.

### DockerSpawner Implementation

**Why DockerSpawner?**
We chose [DockerSpawner](https://jupyterhub-dockerspawner.readthedocs.io) to manage user instances because it provides:

- **Isolation**: Each user runs in their own isolated Docker container, preventing conflicts between users' processes and dependencies.
- **Consistency**: Every user starts with the exact same environment (defined by our Docker image), ensuring reproducibility for deep learning tasks.
- **Flexibility**: It allows us to easily update the environment by rebuilding the image without affecting the host system.

**How it Works**

1.  When a user logs in to JupyterHub, the Hub calls DockerSpawner.
2.  DockerSpawner communicates with the Docker daemon to create a new container for that specific user.
3.  This container runs the `jupyterhub-singleuser` server.
4.  JupyterHub then proxies traffic from the user's browser to their specific container.

**References**
- [JupyterHub Documentation](https://hub.jupyter.org/)
- [DockerSpawner Documentation](https://jupyterhub-dockerspawner.readthedocs.io)
