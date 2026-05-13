# 🚀 Melanie's DevOps Portfolio

> **Cloud-Native Platform Engineering with Kubernetes, GitOps, and Full-Stack Observability**

안녕하세요! DevOps 엔지니어를 지망하는 Melanie입니다.
이 Organization은 클라우드 네이티브 인프라 운영 역량을 증명하기 위한
실전형 포트폴리오 프로젝트입니다.

---

## 🎯 프로젝트 한눈에 보기

Spring Boot 기반 이커머스 마이크로서비스 3개를 AWS EKS 위에 배포하고,
**IaC → CI/CD → GitOps → Observability** 로 이어지는
End-to-End DevOps 파이프라인을 구축했습니다.

### 👉 [전체 개요 & 아키텍처 보러가기](https://github.com/melan-devops1/portfolio-overview)

---

## 📦 Repositories

| Repository | 역할 | 핵심 기술 |
|---|---|---|
| 📘 [portfolio-overview](https://github.com/melan-devops1/portfolio-overview) | 프로젝트 허브: 아키텍처·ADR 인덱스·운영 문서 링크 | Documentation |
| 🌱 [portfolio-app](https://github.com/melan-devops1/portfolio-app) | Spring Boot 마이크로서비스 (product/order/payment) + GitHub Actions CI | Spring Boot 3.5.13, Java 21, Docker, Trivy |
| ☁️ [portfolio-infra](https://github.com/melan-devops1/portfolio-infra) | AWS EKS 인프라 코드 + 운영 문서(SLA/Runbook/Incident) | Terraform 1.14, AWS (VPC/EKS/ECR/RDS) |
| 🚢 [portfolio-manifests](https://github.com/melan-devops1/portfolio-manifests) | GitOps 대상 K8s 매니페스트 | Kustomize, Helm, ArgoCD |

---

## 🛠 Tech Stack

**Infrastructure** · AWS (EKS, VPC, ECR, RDS, IAM, S3) · Terraform · Pod Identity
**Orchestration** · Kubernetes 1.33 · Kustomize (자체 앱) · Helm (3rd party)
**CI/CD** · GitHub Actions (OIDC) · ArgoCD v3.3.6 (auto-sync + self-heal)
**Observability** · Prometheus + Grafana · EFK Stack (Fluent Bit) · Jaeger + OpenTelemetry
**Application** · Spring Boot 3.5.13 · Java 21 · PostgreSQL 15

---

## 🏗 High-Level Architecture

```
Developer Push
      ↓
GitHub Actions (Build · Trivy Scan · Push to ECR · Bump Manifest)
      ↓
Manifest Repo Update (Image Tag → commit-sha)
      ↓
ArgoCD (GitOps Sync — auto + self-heal)
      ↓
AWS EKS Cluster
├── Spring Microservices (product/order/payment + RDS)
├── Prometheus + Grafana + Alertmanager (Metrics)
├── EFK Stack (Logs — Fluent Bit + Elasticsearch + Kibana)
└── Jaeger + OpenTelemetry (Traces)
```

상세 다이어그램과 설계 결정(ADR 29개)은 [portfolio-overview](https://github.com/melan-devops1/portfolio-overview)에서 확인하실 수 있습니다.

---

## 📬 Contact

- 📧 Email: melanie0617@gmail.com
- 📝 Blog: [Notion Blog](https://0617.notion.site/Melanie-s-Devlog-3252455b985d4c0584eb9a94e54a6416?source=copy_link)
