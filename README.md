# air-max-270-gitops

# Technical Report — air-max-270 Observability Stack

**Author:** Yogesh  
**Date:** 12 Sep 2025  
**Repos:**  
- GitOps (K8s + Observability): https://github.com/YOGESHnp/air-max-270-gitops

---

**CD (Argo CD)**
  - App-of-Apps boots the platform (ALB Controller, Loki stack, apps).
  - Auto-sync with prune + self-heal ensures declarative drift correction.
  - **Multi-env** via overlays:
    - `dev`: `latest`, namePrefix `dev-`, dedicated Ingress rule.
    - `staging`: `staging`, namePrefix `stg-`, dedicated Ingress rule (shared ALB).

