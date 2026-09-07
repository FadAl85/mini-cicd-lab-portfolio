# Pipeline Recovery Test

## Objective

The goal of this exercise was to restore the CI/CD pipeline after the intentionally introduced validation failure and verify that the complete delivery workflow operates successfully again.

---

## Previous Failure

During the previous test, the PowerShell validation condition was deliberately changed from:

```powershell
if ($content -notmatch "Mini CI/CD Lab") {
```

to:

```powershell
if ($content -notmatch "THIS_TEXT_DOES_NOT_EXIST") {
```

Because the artificial string did not exist in `index.html`, the PowerShell script executed a `throw` statement and returned exit code `1`.

GitHub Actions correctly stopped the pipeline and skipped all subsequent Docker, GHCR and Kubernetes deployment steps.

---

## Recovery Action

The intentionally incorrect validation condition was restored to its original value:

```powershell
if ($content -notmatch "Mini CI/CD Lab") {
```

The modification was then committed and pushed to the private working repository.

The self-hosted GitHub Actions runner received the new workflow job automatically.

---

## Expected Behavior

After restoring the validation condition, the complete CI/CD workflow was expected to run again:

```text
Code Fix
   |
   v
Git Commit
   |
   v
Git Push
   |
   v
GitHub Actions
   |
   v
PowerShell Test
   |
   v
Kubernetes Connectivity Check
   |
   v
GHCR Authentication
   |
   v
Docker Image Build
   |
   v
Docker Image Push
   |
   v
Kubernetes Deployment Update
   |
   v
Kubernetes Rollout Verification
```

---

## Result

The pipeline completed successfully after the validation condition was restored.

The PowerShell test passed again and the workflow continued with the remaining CI/CD stages.

The successful execution confirmed that:

- the PowerShell validation was working again,
- the self-hosted runner successfully executed the workflow,
- the Kubernetes cluster was reachable,
- authentication to GitHub Container Registry succeeded,
- the Docker image was built,
- the Docker image was pushed to GHCR,
- the Kubernetes deployment was updated,
- the Kubernetes rollout completed successfully.

---

## Verified Recovery

The exercise demonstrated the following recovery process:

```text
Intentional Test Failure
        |
        v
Pipeline Stopped
        |
        v
Failure Identified
        |
        v
Validation Fixed
        |
        v
New Commit and Push
        |
        v
Pipeline Successful
        |
        v
Deployment Restored
```

This confirms that the CI/CD environment can be returned to a working state after a controlled pipeline failure.

---

## Learning Outcome

This exercise provided practical experience with the complete failure-and-recovery cycle of a CI/CD pipeline.

The main lessons were:

- automated tests can act as deployment gates,
- a failing test prevents later delivery stages from executing,
- GitHub Actions logs can be used to identify the failed stage,
- the faulty condition can be corrected and versioned through Git,
- a new push automatically starts another pipeline execution,
- the pipeline resumes normal operation once validation succeeds,
- the self-hosted runner reconnects the GitHub workflow with the local Kubernetes environment.

The combination of the failure test and the recovery test demonstrated both protection against faulty deployments and controlled restoration of the delivery pipeline.

---

## Current Status

The CI/CD pipeline is operational again.

```text
PowerShell Test             PASS
Kubernetes Connectivity     PASS
GHCR Authentication         PASS
Docker Image Build          PASS
Docker Image Push           PASS
Kubernetes Deployment       PASS
Kubernetes Rollout          PASS
```

---

## Next Step

The next planned exercise is a controlled Kubernetes rollback test.

The goal will be to deploy a new version and then practice restoring the previous working Kubernetes revision using rollout history and rollback functionality.