# ArgoCD란?

**ArgoCD**는 Kubernetes 환경에서 사용하는 대표적인 **GitOps 기반 CD(Continuous Delivery) 도구**입니다. 쉽게 말하면:

👉 *“Git에 있는 설정 그대로 Kubernetes를 자동으로 맞춰주는 도구”* 입니다.

---

## 🔷 1. ArgoCD 핵심 개념

### ✅ GitOps란?

* Git 저장소를 **단일 진실(Source of Truth)** 로 사용
* 클러스터 상태 = Git 상태와 항상 동일해야 함

👉 즉,

* Git에 YAML 수정 → 자동으로 Kubernetes 반영

---

## 🔷 2. ArgoCD의 역할

ArgoCD는 다음을 자동으로 처리합니다:

### 📌 1) 배포 자동화

* Git repo에 있는 Kubernetes YAML / Helm / Kustomize 읽음
* 클러스터에 자동 배포

### 📌 2) 상태 동기화 (Sync)

* Git 상태 vs 실제 클러스터 상태 비교
* 다르면 자동 수정

👉 예:

* 누가 kubectl로 몰래 수정 → ArgoCD가 다시 원래대로 복구

### 📌 3) Drift 감지

* “원래 상태와 다른지” 지속적으로 감시

---

## 🔷 3. 구성 요소

### 주요 컴포넌트

| 구성 요소              | 역할                |
| ---------------------- | -----------------   |
| API Server             | UI / CLI / API 제공 |
| Repository Server      | Git repo 가져오기   |
| Application Controller | 실제 상태 동기화    |
| Redis                  | 캐싱                |

---

## 🔷 4. 동작 흐름

```
1. Git에 YAML push
2. ArgoCD가 변경 감지
3. Kubernetes 적용
4. 상태 계속 모니터링
```

---

## 🔷 5. 배포 방식 예시

### Git repo 구조

```
ArgoCD
├── deployment.yaml
├── echo-hostname
│   ├── Dockerfile
│   ├── build.sh
│   ├── main.py
│   └── requirements.txt
└── svc.yaml
```

### ArgoCD Application 생성

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
spec:
  source:
    repoURL: https://github.com/example/repo
    path: my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated: {}
```

👉 이렇게 하면:

* Git 변경 → 자동 배포됨

---

## 🔷 6. 장점

### 🚀 1) 완전 자동화

* CI → 이미지 빌드
* CD → ArgoCD가 자동 배포

### 🔒 2) 변경 이력 관리

* 모든 변경 = Git commit
* 롤백 쉬움

### 👀 3) 가시성

* Web UI 제공 (상태 확인 쉬움)

### 🔁 4) Self-healing

* 설정 틀어지면 자동 복구

---

## 🔷 7. CI/CD에서 위치

```
[CI]
GitLab CI / GitHub Actions
   ↓ (이미지 빌드)
Docker Registry

[CD]
ArgoCD
   ↓
Kubernetes 배포
```

👉 핵심 포인트:

* CI와 CD 분리
* CD는 ArgoCD가 담당

---

## 🔷 8. 언제 쓰는가?

특히 이런 환경에서 필수:

* Kubernetes 운영 환경
* 멀티 클러스터 관리
* MLOps / DevOps 환경
* 자동 배포 + 롤백 필요할 때

---

