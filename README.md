# Teleport Access Config

Personal [Teleport](https://goteleport.com/) configuration for a small home-lab / side-project setup. This is where I experiment with identity-based access management instead of static SSH keys and long-lived credentials.

## What's here

- **`config/roles/`** — RBAC roles for different levels of access (admin, dev, prod, a scoped LLM-project role, and a role for CI/CD deploys), each restricted by node labels and login.
- **`config/bots/`** and **`config/token/`** — Machine ID bot and join-token definitions that let a GitLab CI/CD pipeline authenticate to the cluster without storing static secrets, using Teleport's GitLab join method.
- **`config/users/`** — Example human/service user definitions mapped to the roles above.
- **`tbot.yaml`** — Machine ID (`tbot`) config used by CI to mint a short-lived SSH identity at pipeline runtime.
- **`.gitlab-ci.yml`** — Sample pipeline that uses `tbot` to fetch a short-lived identity and SSH into a target host as part of a deploy job.

## Why

Instead of shipping SSH keys to CI runners or servers, the pipeline authenticates via GitLab's OIDC identity, Teleport issues a short-lived certificate, and access is governed by RBAC roles instead of who has a copy of a key. It's a compact example of certificate-based, short-lived access for both humans and automation.

No live secrets, tokens, or credentials are stored in this repo — join tokens define *who is allowed to join*, not a shared secret.
