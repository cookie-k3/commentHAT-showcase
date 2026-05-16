# commentHAT-showcase

YouTube creator comment analytics platform

댓글 데이터를 기반으로 감정 분석, 카테고리 분석 및 영상 통계를 제공하는 분석 플랫폼입니다.

## Links
- **Service**: [https://commenthat.org/](https://commenthat.org/) (※ 서버 운영 상태에 따라 접속이 제한될 수 있습니다.)
- **Demo Video**: [![Watch Demo](https://img.youtube.com/vi/Uh9heLD3VVw/maxresdefault.jpg)](https://www.youtube.com/watch?v=Uh9heLD3VVw)

---

## 1. 프로젝트 개요

### 🚀 소개
<b>크리에이터 평판 분석 시스템(commentHAT)</b>은 유튜브 채널 데이터와 댓글 데이터를 수집하여 시청자의 반응을 정량적으로 분석하고 콘텐츠를 추천하는 플랫폼입니다. 크리에이터는 이 시스템에서 제공하는 채널 분석 자료와 콘텐츠 가이드를 통해 기획과 제작에 필요한 인사이트를 얻을 수 있습니다.

### 💡 배경 및 필요성
유튜브 시장이 빠르게 성장하면서 크리에이터들은 효율적으로 콘텐츠를 제작하고 시청자들의 반응을 분석하고자 하는 요구가 늘고 있습니다. 그러나 기존 분석 서비스들은 단순한 조회수나 댓글 수치 등 원본 데이터만 제공하여 체계적인 반응 분석과 다음 콘텐츠 기획에는 한계가 있었습니다. 

이에 따라, 대형 언어 모델(LLM)을 통해 댓글 데이터를 심층 분석하고, 다음 영상 제작을 위한 구체적인 가이드를 자동 생성해 주는 시스템을 개발하게 되었습니다.

### 👥 팀원 소개 (팀명: 쿠키크)

| **김미혜 (팀장)** | **강다빈** | **김소정** |
| :---: | :---: | :---: |
| <img src="https://github.com/user-attachments/assets/d8b5b154-21d2-44b4-9935-efe0df094060" width="140" height="170" alt="김미혜"/> | <img src="https://github.com/user-attachments/assets/be8e535a-3345-48b8-bbd4-843e1e7c4148" width="140" height="170" alt="강다빈"/> | <img src="https://github.com/user-attachments/assets/427ffee9-3d6b-4ced-ad4c-c59d13628492" width="140" height="170" alt="김소정"/> |
| Full-Stack Development<br>(FE/BE 개발 전반 참여) | Full-Stack Development<br>(FE/BE 개발 전반 참여) | Full-Stack Development<br>(FE/BE 개발 전반 참여) |
| <b>AutoProcessor</b> 모듈 중점 개발<br>(유튜브 API 연동 및 데이터 자동 수집 파이프라인 구축) | <b>AnalysisService</b> 모듈 중점 개발<br>(AI 모델 연동, 댓글 감성 분석 및 범주화 파이프라인 구현) | <b>ViewService (Front/Back)</b> 중점 개발<br>(사용자 UI/UX 시각화 및 웹 서비스 백엔드 구현) |



## 2. 사용 기술

* **Frontend**: React.js
* **Backend**: Spring Boot 3.2.4 (Java)
* **AI & Data Analysis**: Python, ChatGPT API, KOBERT
* **Database**: MySQL 8.0
* **Message Broker**: RabbitMQ
* **DevOps & Infra**: Kubernetes, GitHub Actions, GitHub Packages, ArgoCD, Grafana, Prometheus



## 3. 아키텍처

### 🏛️ 전체 시스템 아키텍처
<img width="600" alt="System Architecture" src="https://github.com/user-attachments/assets/edc5cc87-2ad2-48e8-9378-d2f133ac8e5f" />

본 시스템은 <b>마이크로서비스 아키텍처(MSA)</b>를 기반으로 설계되어 각 구성 요소가 독립적으로 실행으며, 서비스 간의 비동기 통신을 위해 RabbitMQ 메시지 큐를 활용합니다.
* **ViewService**: 사용자가 접하는 웹 프론트엔드와 백엔드를 포함하며, 분석된 결과 데이터를 데이터베이스에서 조회하여 직관적인 UI로 제공합니다.
* **AutoProcessor**: 주기적인 스케줄링에 따라 유튜브 API를 통해 채널 및 영상 메타데이터, 댓글 데이터를 자동으로 수집하고 정제합니다.
* **AnalysisService**: 수집된 데이터를 바탕으로 KoBERT 분석 및 LLM(ChatGPT API) 파이프라인을 거쳐 댓글의 감성 분석, 범주화, 콘텐츠 가이드 추천 생성을 수행합니다.

### 🔄 CI/CD 및 모니터링 자동화 파이프라인
<img width="600" alt="Automation Pipeline" src="https://github.com/user-attachments/assets/e21cc797-a13e-45b9-bfb7-3deb01fc2c0c" />

안정적인 서비스 운영과 효율적인 배포 관리를 위해 구축된 지속적 통합 및 배포(CI/CD), 모니터링 아키텍처입니다.
1. 개발자가 소스 코드를 **GitHub Repository**에 반영하면 **GitHub Actions**가 트리거되어 자동으로 빌드 및 테스트를 수행합니다.
2. 빌드가 완료된 컨테이너 이미지는 **GitHub Packages**에 저장되며, 배포 구성을 위한 배포 Manifest 파일이 업데이트됩니다.
3. **ArgoCD**가 Manifest 파일의 변경 사항을 감지하여 **Kubernetes 클러스터**의 Pod를 최신 상태로 자동 배포(GitOps)합니다.
4. 클러스터 내에서 실행되는 인프라와 컨테이너의 상태 정보는 **Prometheus**에 의해 수집되며, **Grafana** 대시보드를 통해 실시간 모니터링 및 시각화가 이루어집니다.
