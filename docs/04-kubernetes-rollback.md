# Kubernetes Rollback Test

## Objective

The goal of this exercise was to test Kubernetes deployment rollback functionality and restore a previously working application version.

The application was running as Version 2 before the rollback.

---

## Initial State

The Kubernetes deployment was healthy:

```text
Deployment: mini-cicd
Ready:      1/1
Available:  1
```

The rollout history initially contained two revisions:

```text
Revision 1 → ghcr.io/fadal85/mini-cicd:v1
Revision 2 → ghcr.io/fadal85/mini-cicd:v2
```

The active deployment image was:

```text
ghcr.io/fadal85/mini-cicd:v2
```

The application therefore displayed Version 2.

---

## Inspecting the Rollout History

The deployment history was checked with:

```powershell
kubectl rollout history deployment/mini-cicd
```

Individual revisions were inspected using:

```powershell
kubectl rollout history deployment/mini-cicd --revision=1
kubectl rollout history deployment/mini-cicd --revision=2
```

This confirmed:

```text
Revision 1 → Application Version 1
Revision 2 → Application Version 2
```

---

## Rollback

The deployment was rolled back to the previous Version 1 configuration using:

```powershell
kubectl rollout undo deployment/mini-cicd --to-revision=1
```

Kubernetes returned:

```text
deployment.apps/mini-cicd rolled back
```

The rollback therefore completed successfully.

---

## Kubernetes Warning

During the rollback, Kubernetes displayed a warning that the deployment had previously been managed with:

```text
kubectl apply
```

and that `rollout undo` does not update the stored `last-applied-configuration` annotation.

This warning did not prevent the rollback from succeeding.

It indicates that future `kubectl apply` operations should be handled carefully because the stored declarative configuration may differ from the state created by the rollback.

---

## Revision Behavior After Rollback

An important observation was that Kubernetes did not simply make Revision 1 current again.

Before rollback:

```text
Revision 1 → v1
Revision 2 → v2
```

After rollback:

```text
Revision 2 → v2
Revision 3 → v1
```

The restored Version 1 configuration became a new Kubernetes revision.

This demonstrates that Kubernetes revision numbers represent deployment history, not application version numbers.

Therefore:

```text
Kubernetes Revision 3
```

can contain:

```text
Application Version 1
```

---

## Verification

The new revision was inspected with:

```powershell
kubectl rollout history deployment/mini-cicd --revision=3
```

The output confirmed that Revision 3 uses:

```text
ghcr.io/fadal85/mini-cicd:v1
```

The browser was also opened at:

```text
http://localhost:30080
```

and the application displayed Version 1.

The rollback was therefore verified both through Kubernetes and through the running application.

---

## Result

The rollback test was successful.

```text
Application v2
      |
      v
Kubernetes Revision 2
      |
      v
Rollback to previous configuration
      |
      v
Kubernetes Revision 3
      |
      v
Application v1
```

The previous working application version was restored without manually recreating the deployment.

---

## Learning Outcome

This exercise provided practical experience with:

- Kubernetes rollout history
- inspecting deployment revisions
- identifying the container image used by each revision
- executing a controlled rollback
- verifying the active image after rollback
- understanding the difference between application versions and Kubernetes revisions
- observing how Kubernetes creates a new revision during rollback
- interpreting warnings related to `kubectl apply`
- validating a rollback from both the command line and browser

A key lesson was:

**A Kubernetes revision number describes a deployment state in history and is independent of the application's own version number.**

---

## Current State After Test

At the end of the rollback test:

```text
Kubernetes Revision: 3
Application Image:   ghcr.io/fadal85/mini-cicd:v1
Application Status:  Running
Browser Test:        Successful
```

---

## Next Step

The lab will next be restored from application Version 1 back to Version 2.

This will return the CI/CD environment to its normal current application state before continuing with additional Kubernetes exercises such as health checks and scaling.