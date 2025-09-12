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

### Trade-offs
- **Simplicity vs. HA:** Single NAT/ALB is cost-efficient for demo; production should use **multi-AZ** NAT, ALB subnets, and multiple node groups.
- **Speed vs. least privilege:** For the deadline, the ALB Controller IAM was attached to the **node role**. Production should migrate to **IRSA** with minimal policy.
- **Storage:** Loki persistence disabled for quick bring-up (no EBS CSI). Production should **enable persistence** via the **EBS CSI driver** and a default `gp3` StorageClass.

