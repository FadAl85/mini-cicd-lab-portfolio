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

## Restoration of the Normal Lab State

After successfully verifying the rollback to application Version 1, the lab was restored to the normal current application version.

The Kubernetes deployment image was changed back to:

```text
ghcr.io/fadal85/mini-cicd:v2
```

The rollout completed successfully.

The active deployment image was verified and the application was tested again through:

```text
http://localhost:30080
```

The browser displayed Version 2 successfully.

---

## Revision History After Restoration

After restoring Version 2, the Kubernetes rollout history showed:

```text
Revision 3 → Application Version 1
Revision 4 → Application Version 2
```

This completed the full rollback and recovery cycle:

```text
Revision 1 → v1
Revision 2 → v2
        |
        v
Rollback
        |
        v
Revision 3 → v1
        |
        v
Restore Normal State
        |
        v
Revision 4 → v2
```

This further demonstrated that Kubernetes revision numbers represent deployment history rather than application version numbers.

---

## Final State

At the end of the exercise:

```text
Kubernetes Revision: 4
Application Image:   ghcr.io/fadal85/mini-cicd:v2
Application Status:  Running
Browser Test:        Successful
```

The CI/CD lab was therefore returned to its normal working state after the rollback exercise.

---

## Learning Outcome

The complete exercise demonstrated both sides of Kubernetes deployment recovery:

- inspecting rollout history
- identifying images associated with revisions
- rolling back from Version 2 to Version 1
- understanding creation of a new Kubernetes revision during rollback
- verifying the rollback through Kubernetes and the browser
- restoring the normal Version 2 deployment
- verifying the new post-recovery revision

The lab successfully completed a full:

**Deployment → Rollback → Verification → Restoration**

cycle.

---

## Next Step

The next Kubernetes exercise will focus on application health monitoring using readiness and liveness probes.