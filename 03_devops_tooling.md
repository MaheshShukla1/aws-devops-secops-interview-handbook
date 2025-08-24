# 3) DevOps Tooling

## Section: Terraform

**Q98. What is Terraform state and why is it sensitive?**

**Answer (Natural):**  
State maps real resources to your config. It's sensitive because it can contain secrets and must be consistent to avoid drift and accidental destroys. Store remotely with locking.


👉 **Example:** Use S3 backend + DynamoDB table for state locking.


---

**Q99. Modules & version pinning best practices?**

**Answer (Natural):**  
Create small, reusable modules; pin versions to avoid breaking changes; use semantic versioning and `~>` constraints.


👉 **Example:** Root module calls `vpc` module `~> 3.0`.


---

**Q100. `count` vs `for_each`?**

**Answer (Natural):**  
`count` is index-based; `for_each` maps by keys—safer for add/remove without recreation. Prefer `for_each` for identifiable resources.


👉 **Example:** Create ENIs per subnet with `for_each` keyed by subnet id.


---

**Q101. Plan/apply workflow in CI?**

**Answer (Natural):**  
Format/lint, validate, generate plan artifact, require approval, then apply with workspace/env vars. Store plans as build artifacts.


👉 **Example:** GitHub Actions posts plan as a PR comment for review.


---

**Q102. Handling secrets?**

**Answer (Natural):**  
Never commit secrets to code/state. Use variables from Vault/SSM Parameter Store and `sensitive = true`. Redact in logs.


👉 **Example:** Fetch DB password from SSM during apply.


---

## Section: Ansible

**Q103. Inventory and groups?**

**Answer (Natural):**  
Inventory lists hosts; groups let you target roles/vars per environment. Use dynamic inventory for clouds.


👉 **Example:** Group `[web]` and `[db]` with different vars.


---

**Q104. Idempotence—how ensured?**

**Answer (Natural):**  
Modules are idempotent by design; tasks check current state vs desired state. Avoid raw shell unless necessary.


👉 **Example:** `apt:` module installs only if missing.


---

**Q105. Roles vs Playbooks?**

**Answer (Natural):**  
Roles package tasks/templates/handlers/vars for reuse; playbooks orchestrate roles and tasks across hosts.


👉 **Example:** Role `nginx` reused across services.


---

**Q106. Handlers and notifications?**

**Answer (Natural):**  
Handlers run on change (e.g., restart service). Reduces unnecessary restarts.


👉 **Example:** Template change notifies `restart nginx` handler.


---

## Section: Jenkins

**Q107. Declarative vs Scripted Pipeline?**

**Answer (Natural):**  
Declarative has opinionated, readable syntax with stages/steps; Scripted is Groovy-heavy and flexible. Prefer Declarative for team readability.


👉 **Example:** Use shared libraries for common steps.


---

**Q108. Agents and executors?**

**Answer (Natural):**  
Agents provide environments to run builds; executors are concurrency slots. Scale with Kubernetes agents for on-demand builds.


👉 **Example:** Spin ephemeral agents per build in EKS.


---

**Q109. Credentials management?**

**Answer (Natural):**  
Store secrets in Jenkins Credentials or an external vault; never hardcode. Inject only in needed stages.


👉 **Example:** Use withCredentials to scope secrets.


---

## Section: Docker

**Q110. Image vs Container vs Layer?**

**Answer (Natural):**  
Image is a read-only template; container is a running instance; layers stack via union FS. Smaller layers mean faster pulls.


👉 **Example:** Multi-stage builds to keep runtime images minimal.


---

**Q111. ENTRYPOINT vs CMD?**

**Answer (Natural):**  
ENTRYPOINT is the executable; CMD provides default args. Combine for flexibility.


👉 **Example:** ENTRYPOINT ["nginx"] + CMD ["-g","daemon off;"]


---

**Q112. How to reduce image size and CVEs?**

**Answer (Natural):**  
Use slim/base images, multi-stage builds, pin versions, run as non-root, scan images regularly.


👉 **Example:** FROM distroless/static for Go binaries.


---

**Q113. Resource limits and healthchecks?**

**Answer (Natural):**  
Set CPU/memory limits and `HEALTHCHECK` to detect failures. Avoid noisy neighbor effects.


👉 **Example:** HEALTHCHECK curl localhost:8080/healthz || exit 1


---

## Section: Kubernetes & Helm

**Q114. Deployment vs StatefulSet vs DaemonSet?**

**Answer (Natural):**  
Deployment: stateless replicated pods; StatefulSet: stable IDs and storage; DaemonSet: one pod per node for agents. Choose based on state and placement needs.


👉 **Example:** Logs agent as DaemonSet, DB as StatefulSet.


---

**Q115. Readiness vs Liveness probes?**

**Answer (Natural):**  
Readiness gates traffic until app is ready; Liveness restarts unhealthy containers. Proper probes improve resiliency and rolling updates.


👉 **Example:** Readiness checks DB connectivity; Liveness checks internal health.


---

**Q116. Requests & Limits—why set them?**

**Answer (Natural):**  
Scheduler uses **requests**; **limits** cap usage. Set to realistic values to avoid CPU throttling and OOMKills.


👉 **Example:** Baseline CPU 200m/ limit 500m for API pods.


---

**Q117. ConfigMap vs Secret?**

**Answer (Natural):**  
ConfigMap for non-sensitive config; Secret for sensitive (base64-encoded; use KMS/SealedSecrets/External Secrets for encryption at rest).


👉 **Example:** DB creds via Secret + CSI driver for KMS-encrypted volumes.


---

**Q118. Service types: ClusterIP, NodePort, LoadBalancer, Ingress?**

**Answer (Natural):**  
ClusterIP exposes in-cluster; NodePort opens static port on nodes; LoadBalancer provisions external LB; Ingress routes HTTP with host/path rules.


👉 **Example:** Public API via Ingress + ALB controller.


---

**Q119. RollingUpdate vs Recreate?**

**Answer (Natural):**  
RollingUpdate updates with zero/low downtime; Recreate kills all then starts new—use for stateful incompatible upgrades.


👉 **Example:** Set `maxUnavailable` and `maxSurge` thoughtfully.


---

**Q120. Helm charts & values—how to organize?**

**Answer (Natural):**  
Keep charts small; override via env-specific values files; template with functions; pin chart/app versions.


👉 **Example:** `helmfile` or GitOps (ArgoCD) for multi-env syncing.


---

## Section: Git

**Q121. Rebase vs Merge—when to use?**

**Answer (Natural):**  
Merge preserves history; rebase creates linear history. Rebase your feature on main; merge when integrating back to preserve context.


👉 **Example:** Use `git rebase -i` to squash fixups before PR.


---

**Q122. Gitflow vs Trunk-Based Development?**

**Answer (Natural):**  
Gitflow uses long-lived branches and release branches; Trunk-based uses short-lived feature branches and continuous integration—better for CI/CD speed.


👉 **Example:** High-velocity teams prefer trunk + feature flags.


---

**Q123. Submodules vs Monorepo?**

**Answer (Natural):**  
Submodules link external repos; monorepo keeps projects together for shared tooling. Pick based on autonomy and coupling.


👉 **Example:** Org-wide linting and versioning in monorepo.


---
