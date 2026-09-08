# Mini CI/CD Lab

A hands-on CI/CD lab built on Windows 11 to practice a complete container-based software delivery workflow using GitHub Actions, Docker, GitHub Container Registry, a self-hosted runner, and Kubernetes.

## Project Goal

The purpose of this project is to gain practical experience with a complete CI/CD workflow rather than only studying the concepts theoretically.

The lab demonstrates how a code change can move through:

**Git → GitHub → Automated Test → Docker Build → Container Registry → Kubernetes Deployment**

The application itself is intentionally small so that the focus remains on CI/CD, containerization, automation, deployment, and troubleshooting.

---

## Architecture


```text
Developer / VS Code
        |
        v
       Git
        |
        v
      GitHub
        |
        v
  GitHub Actions
        |
        +---- Automated PowerShell Test
        |
        +---- Docker Image Build
        |
        v
GitHub Container Registry (GHCR)
        |
        v
Self-hosted GitHub Actions Runner
        |
        v
      kubectl
        |
        v
Local Kubernetes Cluster
        |
        v
    Deployment
        |
        v
       Pod
        |
        v
Nginx Container + Web Application
        |
        v
 localhost:30080
```

## Documentation

Detailed technical documentation is available here:

- [Lab Setup and Baseline](docs/01-lab-setup-and-baseline.md)
- [Pipeline Failure Test](docs/02-pipeline-failure-test.md)
- [Pipeline Recovery Test](docs/03-pipeline-recovery.md)