# 🚀 Melanie's DevOps Portfolio

> **Cloud-Native Platform Engineering with Kubernetes, GitOps, and Full-Stack Observability**

안녕하세요! DevOps 엔지니어를 지망하는 Melanie입니다.
이 Organization은 클라우드 네이티브 인프라 운영 역량을 증명하기 위한
실전형 포트폴리오 프로젝트입니다.

---

## 🎯 프로젝트 한눈에 보기

Spring Boot 기반 이커머스 마이크로서비스를 AWS EKS 위에 배포하고,
**IaC → CI/CD → GitOps → Observability → Service Mesh** 로 이어지는
End-to-End DevOps 파이프라인을 구축했습니다.

### 👉 [전체 개요 & 아키텍처 보러가기](https://github.com/melan-devops1/portfolio-overview)

---

## 📦 Repositories

| Repository | 역할 | 핵심 기술 |
|---|---|---|
| 📘 [portfolio-overview](https://github.com/melan-devops1/portfolio-overview) | 프로젝트 허브: 아키텍처·ADR·트러블슈팅 | Documentation |
| 🌱 [portfolio-app](https://github.com/melan-devops1/portfolio-app) | Spring Boot 마이크로서비스 (product/order/payment) | Spring Boot 3, Java 17, Docker |
| ☁️ [portfolio-infra](https://github.com/melan-devops1/portfolio-infra) | AWS EKS 인프라 코드 | Terraform, AWS VPC/EKS/ECR |
| 🚢 [portfolio-manifests](https://github.com/melan-devops1/portfolio-manifests) | GitOps 대상 K8s 매니페스트 | Helm, Kustomize, ArgoCD |

---

## 🛠 Tech Stack

**Infrastructure** · AWS (EKS, VPC, ECR, IAM, S3) · Terraform
**Orchestration** · Kubernetes · Helm · Kustomize
**CI/CD** · GitHub Actions · ArgoCD (GitOps)
**Observability** · Prometheus · Grafana · EFK Stack · Jaeger
**Service Mesh** · Istio · Nginx Ingress Controller
**Application** · Spring Boot 3 · PostgreSQL

---

## 🏗 High-Level Architecture

```
Developer Push
      ↓
GitHub Actions (Build · Test · Trivy Scan · Push to ECR)
      ↓
Manifest Repo Update (Image Tag Bump)
      ↓
ArgoCD (GitOps Sync)
      ↓
AWS EKS Cluster
├── Istio Service Mesh (mTLS, Canary)
├── Spring Microservices (product/order/payment)
├── Prometheus + Grafana (Metrics)
├── EFK Stack (Logs)
└── Jaeger (Tracing)
```

상세 다이어그램과 설계 결정은 [portfolio-overview](https://github.com/melan-devops1/portfolio-overview) 에서 확인하실 수 있습니다.

---

## 📬 Contact

- 📧 Email: melanie0617@gmail.com
- 💼 LinkedIn: [linkedin.com/in/melanie-woo](https://www.linkedin.com/in/melanie-woo-32a268229/)
- 📝 Blog: [Notion Blog](https://0617.notion.site/Melanie-s-Devlog-3252455b985d4c0584eb9a94e54a6416?source=copy_link)
