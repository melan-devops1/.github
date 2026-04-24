# 📘 DevOps Portfolio - Project Overview

<!-- TODO(Phase 2): 아키텍처 다이어그램 이미지 추가
<p align="center">
  <img src="./docs/diagrams/architecture.png" alt="Architecture" width="800"/>
</p>
-->

<p align="center">
  <a href="https://github.com/melan-devops1/portfolio-app"><img src="https://img.shields.io/badge/App-Spring_Boot-6DB33F?logo=springboot&logoColor=white"/></a>
  <a href="https://github.com/melan-devops1/portfolio-infra"><img src="https://img.shields.io/badge/Infra-Terraform-7B42BC?logo=terraform&logoColor=white"/></a>
  <a href="https://github.com/melan-devops1/portfolio-manifests"><img src="https://img.shields.io/badge/GitOps-ArgoCD-EF7B4D?logo=argo&logoColor=white"/></a>
  <img src="https://img.shields.io/badge/Cloud-AWS-232F3E?logo=amazonwebservices&logoColor=white"/>
  <img src="https://img.shields.io/badge/Orchestration-Kubernetes-326CE5?logo=kubernetes&logoColor=white"/>
</p>

> 🚧 **진행 중인 프로젝트입니다.** 각 Phase가 완료되는 대로 문서·다이어그램·스크린샷이 업데이트됩니다.

---

## 🎯 프로젝트 목표

이커머스 플랫폼을 가정한 마이크로서비스를 **AWS EKS 위에서 운영하는 실전형 DevOps 플랫폼**을
처음부터 끝까지 혼자 구축한 1인 프로젝트입니다.

핵심 질문은 다음과 같았습니다:

- 🔹 인프라를 코드로(IaC) 완전히 재현 가능하게 만들 수 있는가?
- 🔹 개발자가 `git push` 한 번으로 프로덕션까지 배포되는 파이프라인을 만들 수 있는가?
- 🔹 장애가 발생했을 때 **메트릭·로그·트레이스** 세 축으로 원인을 추적할 수 있는가?
- 🔹 SLA 99.9%를 지키기 위한 알림·에스컬레이션 체계를 수립할 수 있는가?

---

## 📦 Repository Map

| Repository | 설명 |
|---|---|
| **[portfolio-app](https://github.com/melan-devops1/portfolio-app)** | 3개의 Spring Boot 마이크로서비스 (product/order/payment). Dockerfile, 로컬 docker-compose 포함 |
| **[portfolio-infra](https://github.com/melan-devops1/portfolio-infra)** | Terraform 모듈 기반 AWS 인프라 코드 (VPC, EKS, ECR, IAM). S3 Remote State + DynamoDB Lock |
| **[portfolio-manifests](https://github.com/melan-devops1/portfolio-manifests)** | ArgoCD가 watch하는 GitOps 레포. Helm + Kustomize로 환경별 배포 구성 |

---

## 🏗 Architecture

### End-to-End Flow

```
┌─────────────┐      ┌──────────────────┐       ┌──────────────────┐
│  Developer  │─────▶│  GitHub (App)    │─────▶│ GitHub Actions  │
└─────────────┘      └──────────────────┘       │  - Build         │
                                                │  - Test          │
                                                │  - Trivy Scan    │
                                                │  - Push to ECR   │
                                                │  - Bump Manifest │
                                                └────────┬─────────┘
                                                         │
                                                         ▼
                                                ┌──────────────────┐
                                                │ GitHub(Manifests)│
                                                └────────┬─────────┘
                                                         │ watch
                                                         ▼
┌──────────────────────────────────────────────┐   ┌──────────────┐
│              AWS EKS Cluster                 │◀─│   ArgoCD     │
│                                              │   └──────────────┘
│  ┌────────────────────────────────────────┐  │
│  │         Istio Service Mesh             │  │
│  │  ┌───────┐   ┌──────┐   ┌──────────┐   │  │
│  │  │product│─▶│ order│─▶│ payment  │   │  │
│  │  └───────┘   └──────┘   └──────────┘   │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌──────────────┐ ┌──────────────┐           │
│  │  Prometheus  │ │  EFK Stack   │           │
│  │  + Grafana   │ │  (Fluent Bit)│           │
│  └──────────────┘ └──────────────┘           │
│                                              │
│  ┌──────────────┐ ┌──────────────┐           │
│  │    Jaeger    │ │ Alertmanager │──▶ Slack │
│  └──────────────┘ └──────────────┘           │
└──────────────────────────────────────────────┘
```

> 🚧 시각화된 다이어그램은 Phase 2(인프라 설계) 완료 시점에 추가됩니다.

---

## 🛠 Tech Stack

### Infrastructure & Cloud
- **AWS**: EKS, VPC, ECR, IAM, S3, DynamoDB, Route53
- **IaC**: Terraform (modules, remote state, workspace 분리)

### Orchestration & Deployment
- **Kubernetes** 1.29
- **Packaging**: Helm 3, Kustomize (overlays: dev/prod)
- **CI**: GitHub Actions (OIDC 기반 AWS 인증)
- **CD / GitOps**: ArgoCD (App-of-Apps 패턴)

### Observability (3-Pillar)
- **Metrics**: Prometheus + Grafana (kube-prometheus-stack)
- **Logs**: Elasticsearch + Fluent Bit + Kibana
- **Traces**: Jaeger + OpenTelemetry

### Network & Security
- **Ingress**: Nginx Ingress Controller
- **Service Mesh**: Istio (mTLS STRICT, VirtualService 기반 Canary)
- **Security**: Trivy 이미지 스캔, IRSA(IAM Roles for Service Accounts)

### Application
- **Spring Boot** 3.2, **Java** 17
- **DB**: PostgreSQL 15

---

## 📊 Key Achievements (운영 수치)

> 실제 구축/측정이 끝나는 대로 숫자를 채워 넣습니다. 각 항목은 관련 문서/커밋으로 연결됩니다.

- 🚧 Fluentd → Fluent Bit 전환: DaemonSet 메모리 사용량 **__%** 절감
- 🚧 이미지 최적화: 기본 빌드 대비 최종 이미지 크기 **__%** 감소
- 🚧 CI 파이프라인 실행 시간: **__초** (Layered JAR + 캐싱)
- 🚧 배포 리드 타임 (commit → prod): **__분**
- 🚧 SLA 산출 기간 동안 가용성: **__%** (목표 99.9%)

---

## 📝 주요 설계 결정 (ADR)

운영 환경에서 내린 기술 선택들을 ADR(Architecture Decision Record)로 정리할 예정입니다.

- 🚧 ADR-0001: 마이크로서비스 분리 구조 *(작성 예정)*
- 🚧 ADR-0002: EKS vs Self-Managed Kubernetes *(작성 예정)*
- 🚧 ADR-0003: ArgoCD vs Flux *(작성 예정)*
- 🚧 ADR-0004: Fluentd → Fluent Bit 전환 근거 *(작성 예정)*
- 🚧 ADR-0005: Istio vs Linkerd *(작성 예정)*

---

## 🔧 Troubleshooting Notes

구축 중 마주친 실제 이슈와 해결 과정이 이 섹션에 기록됩니다. *(진행 중)*

---

## 🎬 Screenshots

*(Phase 4 Observability 구축 완료 후 Grafana / Kibana / ArgoCD 스크린샷 추가)*

---

## 🚦 Project Roadmap

- [x] Phase 0: 기반 세팅 (Organization, 레포, 로컬 환경)
- [ ] Phase 1: Spring 앱 개발 + 컨테이너화
- [ ] Phase 2: Terraform 기반 AWS EKS 프로비저닝
- [ ] Phase 3: CI/CD + ArgoCD GitOps
- [ ] Phase 4: Observability 풀스택
- [ ] Phase 5: Istio Service Mesh + 장애 대응
- [ ] Phase 6: 문서화 + 블로그 포스팅

---

## 📬 Contact

- 📧 Email: melanie0617@gmail.com
- 💼 LinkedIn: [linkedin.com/in/melanie-woo](https://www.linkedin.com/in/melanie-woo-32a268229/)
- 📝 Blog: [Notion Blog](https://0617.notion.site/Melanie-s-Devlog-3252455b985d4c0584eb9a94e54a6416?source=copy_link)
