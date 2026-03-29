# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Engineering thesis ("Zaprojektowanie i stworzenie systemu rozwiązań hostingowych i chmurowych") — a cloud hosting platform with three service tiers: web hosting, VPS, and cloud file storage. Documentation is in Polish. Currently in design phase — no source code yet.

## Technology Stack

| Layer | Technology |
|---|---|
| Public sales site | WordPress + WooCommerce |
| Client panel backend | Symfony 7.4 (Doctrine ORM, Symfony Messenger, Symfony Security) |
| Client panel frontend | React + TailwindCSS |
| Platform database | PostgreSQL (panel data + all service metadata) |
| Customer databases | PostgreSQL, MySQL, MongoDB (customer's choice per service) |
| Web hosting | Docker containers orchestrated by Kubernetes |
| VPS | KVM virtual machines managed by Proxmox VE (libvirt/QEMU) |
| Object storage | MinIO (S3-compatible) |
| Email | Postfix + Dovecot + Roundcube |
| DNS | PowerDNS (MySQL backend) |
| Monitoring | Prometheus + Grafana |
| Queue/messaging | RabbitMQ |
| Reverse proxy | Traefik |
| Backup | Restic |
| CI/CD | GitLab CI/CD |
| SSL | Let's Encrypt |

## Architecture

**Three service types**, each with distinct infrastructure:

- **Web Hosting**: Per-customer Docker Compose stacks (nginx-proxy + php-fpm + database + phpmyadmin), orchestrated by Kubernetes. Resource limits enforced via Docker. Network isolation via Docker networks.
- **VPS**: Full KVM virtual machines provisioned via Proxmox VE API. Cloud-init templates for automated OS setup. VNC/noVNC for browser-based console access.
- **File Storage**: MinIO with per-user buckets and access policies. Web UI with drag-and-drop upload.

**Command execution**: Privileged API daemon pattern — the Symfony web process never runs privileged commands directly. Operations are queued via RabbitMQ (Symfony Messenger) and executed by a background worker with appropriate permissions.

## Caveats

- `cloud-hosting/panel-architecture.md` contains early brainstorming that references **Laravel** code examples and **Vue.js**. The actual choices are **Symfony 7.4** and **React**. When generating code, use Symfony patterns (Doctrine, Messenger, Security component), not Laravel.
- Documentation and thesis text are in Polish. Code identifiers should be in English.

## Documentation Map

- `cloud-hosting/Oryginał.md` — thesis text (requirements ch. 2, tech stack ch. 3, architecture ch. 4)
- `cloud-hosting/panel-architecture.md` — detailed architecture brainstorm (contains outdated "or" options — see Caveats)
- `cloud-hosting/uml architektura systemu.md` — PlantUML component diagram (work in progress)
- `cloud-hosting/Kroki jakie wykonuję.md` — implementation steps log
- Root `.docx` — formal thesis document for submission

## Target Infrastructure

Hetzner dedicated server: 8GB RAM, 80GB storage, Debian 13, Proxmox VE.

## Commands

No source code exists yet. When implementation begins, expected tooling:

- `symfony console` — backend CLI commands
- `npm run dev` / `npm run build` — React frontend
- `docker compose` — local development environment
- `kubectl` — Kubernetes cluster management
- `ansible-playbook` — infrastructure deployment
