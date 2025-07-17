# 🌐 AWS 인프라 및 EKS 클러스터 자동화 프로젝트

📁 디렉토리 구조
infra-eks-terraform/
├── terraform/
│   ├── base/        # VPC, RDS, EC2 등 베이스 인프라
│   └── eks/         # EKS 클러스터 및 노드그룹
├── .github/
│   └── workflows/
│       ├── terraform-base.yaml
│       └── terraform-eks.yaml
├── README.md

## 📌 개요
Terraform과 GitHub Actions를 활용하여 AWS 클라우드 환경에  VPC, 서브넷, RDS, 관리 서버, EKS 클러스터, NodeGroup등 인프라를 코드로 자동화.

## ⚙️ 사용 기술

- Terraform 1.6.6
- AWS VPC / RDS / EC2 / EKS
- GitHub Actions (CI 자동화)

## 🧱 구성 요소

| 구성단계          | 설명                                            |
|------------------|------------------------------------------------|                                          
| `terraform/base` | VPC, 서브넷, NATGW, 관리용 EC2, RDS 등 인프라 구성 |
| `terraform/eks`  | EKS 클러스터와 NodeGroup 분리 구성                |
| CI Workflow      | GitHub Actions로 Terraform Plan/Apply자동화     |

## 🔄 자동화 흐름 (GitHub Actions)

### ✅ `Terraform CI/CD - Base`

- `terraform/base/**` 경로에 변경사항이 있을 때 실행
- Terraform Plan → Apply 순으로 인프라 구성 수행
- `RDS_PASSWORD`는 GitHub Secrets로  관리

### ✅ `Terraform CI/CD - EKS`

- 위 `Base` 워크플로우가 성공적으로 완료되었을 때 실행됨
- Step 1: EKS 클러스터 생성
  - 실패 시 자동으로 `terraform destroy` 수행
- Step 2: NodeGroup 생성
  - 실패 시 자동 destroy

### 🔐 GitHub Secrets 요구 사항

| 키                     | 설명                          |
|------------------------|------------------------------|
| `AWS_ACCESS_KEY_ID`    | AWS 액세스 키 ID              |
| `AWS_SECRET_ACCESS_KEY`| AWS 시크릿 키                 |
| `RDS_PASSWORD`         | RDS 인스턴스 루트 비밀번호      |

