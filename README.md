# ssd-k8s-manifests

SSD MSA 서비스의 Kubernetes 배포 자산을 관리하는 저장소다.

## 목표

- Helm 기반 공통 배포 템플릿을 사용한다.
- 서비스별 `values` 파일로 이미지, 포트, 환경 변수, 인그레스 경로를 분리한다.
- dev 환경 기준으로 EKS 배포가 가능하도록 구성한다.

## 구조

- `charts/ssd-service`
  공통 Spring Boot 서비스용 Helm chart
- `environments/dev/services`
  서비스별 values 파일

## 배포 흐름

1. 서비스 저장소가 GitHub Actions로 이미지를 ECR에 푸시한다.
2. 이 저장소의 values 파일에서 이미지 태그를 갱신한다.
3. Helm upgrade 명령으로 EKS에 반영한다.

## 현재 dev 검증 기준

- `ssd-api-gateway`는 현재 `Service type=LoadBalancer`로 외부 노출한다.
- 외부 접근 포트는 `80`, 컨테이너 포트는 `8080`이다.
- 내부 서비스(`auth`, `member`, `document`, `review`, `ai`)는 `ClusterIP`로만 노출한다.
- ALB Ingress Controller와 External Secrets Operator는 후속 단계에서 붙일 수 있도록 값과 예시 매니페스트를 남겨두었다.

## 추가 자산

- `namespaces/dev.yaml`
  dev namespace 정의
- `externalsecrets/`
  서비스별 Secrets Manager 연동 예시
- `.github/workflows/deploy-dev.yml`
  workflow_dispatch 기반 수동 Helm 배포 예시
