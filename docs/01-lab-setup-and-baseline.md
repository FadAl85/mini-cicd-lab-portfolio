# Lab Setup and Baseline

## Purpose

This document describes the initial setup and working baseline of the Mini CI/CD Lab.

The goal of the lab is to build and understand a complete container-based CI/CD workflow on a Windows workstation without maintaining a separate application virtual machine.

The application itself is intentionally simple. The main focus is on:

- version control
- automated testing
- containerization
- container registries
- CI/CD automation
- self-hosted runners
- Kubernetes deployment
- troubleshooting and recovery

---

## Host Environment

The lab is operated on a Windows 11 laptop.

Main tools used:

- Windows 11
- Visual Studio Code
- PowerShell 7
- Git
- Docker Desktop
- WSL2
- GitHub
- GitHub Actions
- GitHub Container Registry
- Kubernetes
- kubectl

No manually administered Ubuntu or Windows application VM was required for this lab.

---

## Why WSL2?

The application runs inside Linux-based containers.

Linux containers require a Linux kernel. Since the host operating system is Windows, WSL2 provides the required Linux environment used by Docker Desktop.

Simplified architecture:

```text
Windows 11
    |
    v
Docker Desktop
    |
    v
WSL2 / Linux environment
    |
    v
Containers and Kubernetes