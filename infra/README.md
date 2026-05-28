# infra/

Placeholder for infrastructure-as-code. **Boring tech by default.** No Kubernetes unless a specific lab is about Kubernetes.

## Subfolders

- [`docker/`](docker/) — Dockerfiles and docker-compose files for labs
- [`terraform/`](terraform/) — IaC for cloud resources (when needed)
- [`kubernetes/`](kubernetes/) — manifests (only if explicitly justified)
- [`local-dev/`](local-dev/) — local development environment helpers

## Default posture

- Local-first: every lab should run with `docker compose up`.
- Cloud only when a lab specifically explores cloud-only behavior.
- Kubernetes only when a lab is explicitly about Kubernetes.
