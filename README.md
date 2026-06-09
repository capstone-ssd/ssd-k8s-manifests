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

## 추가 자산

- `namespaces/dev.yaml`
  dev namespace 정의
- `externalsecrets/`
  서비스별 Secrets Manager 연동 예시
- `.github/workflows/deploy-dev.yml`
  workflow_dispatch 기반 수동 Helm 배포 예시
