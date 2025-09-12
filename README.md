# air-max-270-gitops

### Trade-offs
- **Simplicity vs. HA:** Single NAT/ALB is cost-efficient for demo; production should use **multi-AZ** NAT, ALB subnets, and multiple node groups.
- **Speed vs. least privilege:** For the deadline, the ALB Controller IAM was attached to the **node role**. Production should migrate to **IRSA** with minimal policy.
- **Storage:** Loki persistence disabled for quick bring-up (no EBS CSI). Production should **enable persistence** via the **EBS CSI driver** and a default `gp3` StorageClass.

## Security Rationale

- **AWS IAM**
  - **GitHub OIDC** role for CI: push to ECR without long-lived keys.
  - **Node role**: EC2/ECR read; temporary ELB policy attached for ALB controller.  
    **Roadmap:** move controller to **IRSA** with the official minimal policy; detach broad policies from node role.
  - **Cluster role**: EKS service/control-plane policies.
- **Network**
  - Nodes in **private subnets** (no public IPs); ALB in public subnets.
  - Security groups restrict control plane traffic (443) and kubelet (10250) appropriately.
- **Secrets**
  - No plaintext secrets in Git; Grafana admin creds stored as K8s Secret from Helm.  
  - **Roadmap:** External secrets via AWS Secrets Manager; Argo CD sops.
- **Supply Chain**
  - CI builds/pushes to **ECR**; Kustomize overlays pin tags per environment.  
  - **Roadmap:** image signing/verification (Sigstore), admission policy (OPA/Gatekeeper/Kyverno).
- **State**
  - **Roadmap:** remote Terraform state in S3 with DynamoDB locking and encryption.