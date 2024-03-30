[핵심 기술 및 주요 업무]<br>
- shell, batch, Powershell, 파이썬 스크립트 자동화 작업할 수 있습니다.
- Unity/Unreal 프로젝트 CI/CD 구축할 수 있습니다.
- 서버(java ) docker 환경 CI/CD 구축할 수 있습니다.
- windows, MAC, Linux OS 상관없이 인프라 구성 및 운영할 수 있습니다.
- Grafana + Promehteus + alertmanager 구축하여 모니터링 환경 구성 및 운영할 수 있습니다.
- 다양한 버전관리툴 구축 및 운영할 수 있습니다. ( gitlab, svn, mercurial, perforce...)
- 빌드 자동화에 필요한 툴 서버 구축 및 운영할 수 있습니다. (Jenkins, samba, incredibuild, resource hacker ...)
- DNS, SSL 관리
- 네트워크, 정보보안 지식 (조금) 있습니다. ( 정보 보안 기사 필기 합격)

---

[주요 성과]<br>
- 게임 서버, 게임 빌드 환경 및 빌드 + 배포 구축(CI/CD)
- 모니터링 시스템 구축 및 운영
- 서버 관리와 설치 스크립트 작성 + 자동화 
- 젠킨스 pipeline style 변환
- 버전관리 서버(svn, gitlab) 백업 구축 및 관리
- 다양한 환경(MAC, WIN, Linux)에서 빌드 CI/CD 구축
- 빌드 에러 처리

---

[이직사유]<br>
- 피드백이 없이 혼자서 다 처리하는 상황
- devops 직무에 필수라고 생각하는 k8s와 클라우드 환경의 적극적인 사용 불가
- 아침에 갑자기 권고사직 후, 진행되는 과정을 봐서 직장생활에 불안감을 느낌

---

[CI/CD 구축 경험]<br>
- CI/CD 툴로는 Jenkins, gitlab 사용하고 있습니다.
자동화 내용은 프로젝트 빌드위한 인프라 환경 구축부터  게임 및 서버 빌드, 알람, 프로그램 인증서 등록, 배포 등 진행하고 있습니다.

---

[클라우드 사용 경험]<br>
- 넷마블에서 제공하는 openstack으로만들어진 서비스를 이용하며,  VM 인스턴스 생성 및 harbor 운영 사용합니다.
그후 요청하신 환경에 따라 필요한 설정이나 서버 설치 처리하였습니다.
SVN, gitlab, samba 서버 등이 여기에 해당합니다.

- AWS의 경우 CDN만 사용하여, 배포용으로만 사용 중입니다.

- GCP 경우 DNS, VM 인스턴스, big query, 버킷, cloud functions 위주로 사용하였습니다.
여러 서버에서의 데이터 저장 및 분석을 위해 빌드 종료 후 필요한 데이터를 JSON 형태로 만들어
post로 cloud function에 전달하면, big query에 원하는 형태로 저장하고, 
해당 big query에 있는 데이터를 grafana에 연결하여 그래프로 시각화 사용하고 있습니다.

---

[인프라 경험]<br>
- 주로 이용하는 환경은 구글 클라우드와 openstack 으로 만들어진 서비스 이용하고 있습니다.
간단한 서버 생성부터 SVN, gitlab 서버 설치 및 배포까지 관리하고 있습니다.
인프라 쪽 주 업무는 게임 개발, 게임 서버 환경 세팅입니다. 
환경 구성부터 게임빌드, 배포(CI/CD)까지 하고 있습니다.
그 외에는 SVN, mercurial 등 버전관리 서버 이관, SSL 관리, 서버 ldap 사용자 관리, 서버 모니터링 등 전반적으로 관리하고 있습니다.

---

[docker 나 k8s 사용경험]<br>
- docker의 경우는 java 기반의 WAS(Wep application server) CI/CD 만들었습니다.

- k8s는 grafana + Prometheus + alert manager 모니터링 환경을 k8s 와 helm 이용하여 자동화 R&D 하였다가, 중단한 상태입니다.

---

[개발 경험]<br>
- 24년 3월기준 게임 서벌 개발자도 겸직하고 있습니다. (Spring boot 기반)

---

[구축한 CI/CD 아키텍처]

![image](https://github.com/sungmin4036/private_study/assets/62640332/a5550a48-7fa5-4b88-a5c8-229b063d10c2)

---

![image](https://github.com/sungmin4036/Devops/assets/62640332/84e5c916-bde8-42a3-936e-9200a9eb991c)

---
gitlab pipeline

![image](https://github.com/sungmin4036/Devops/assets/62640332/8f880a70-4715-4972-83fe-b7aea95232c8)

---

jenkins pipeline

![image](https://github.com/sungmin4036/book/assets/62640332/272601d8-bd66-476a-97a2-f4c63c5de8f8)

위처럼 Jenkins pipeline 이나 gitlab pipeline 으로 CI/CD 구현해서 사용하고있습니다.

그리고 파이썬 사용하여 JSON 형식으로 읽어 처리하고있습니다.

아래는 CI/CD 예시 입니다.

```
{
  "common_param": {
    "PROJECT_PATH": "/Volumes/SSD2/repo/git_playworld_unity_android_fqa/",
    "DEPLOY_PATH": "/build/pw/fqa/Android/",
    "EXPORT_PATH": "#common.PROJECT_PATH#Bin/AOS/",
    "SYMBOL_PATH": "#common.PROJECT_PATH#Library/Bee/artifacts/Android/il2cppOutput/build",
    "SDK_PATH": "/Applications/Unity/Hub/Editor/2021.3.15f1/PlaybackEngines/AndroidPlayer/SDK/build-tools/30.0.2",
    "ADD_DEFINE": "BUNDLE_PATH;${TARGET_SERVER}",
    "BRANCH_PATH": "fqa",
    "_IS_RECORD_GRAFANA": true,
    "_IS_USE_SHARED_CONF": true,
    "_ARTIFACT_PATH": "#common.EXPORT_PATH#GrandCrossW_#return.CUR_REV#_symbols.zip",
    "LINE_CHANNEL": "pw_build",
    "STARK_SERVICE_KEY": "PW_SERVICE_KEY",
    "PROJECT_KEY": "PW"
  },
  "commands": [
    {
      "command": "SendJenkinsMessageStart",
      "params": {
        "channel": "#common.LINE_CHANNEL#",
        "stark_service_key": "#common.STARK_SERVICE_KEY#",
        "project_key": "#common.PROJECT_KEY#"
      }
    },
    {
      "command": "Remove",
      "params": {
        "Condition": "${IS_CLEAR}",
        "path": [
          "#common.PROJECT_PATH#/Library"
        ]
      }
    },
    {
      "command": "GetIsEmpty",
      "params": {
        "check_str": "${GIT_TAG}"
      },
      "return": [
        "IS_EMPTY"
      ]
    },
    {
      "command": "GitCleanPullSwitch",
      "params": {
        "Condition": "#return.IS_EMPTY#",
        "project_path": "#common.PROJECT_PATH#",
        "branch": "${BRANCH}",
        "option": [
          "-q"
        ]
      }
    },
    {
      "command": "GitTagCheckout",
      "params": {
        "Condition": "#return.IS_EMPTY# == false",
        "project_path": "#common.PROJECT_PATH#",
        "git_tag": "${GIT_TAG}",
        "branch": "${BRANCH}"
      }
    },
    {
      "command": "GetCurrentGitHash",
      "params": {
        "project_path": "#common.PROJECT_PATH#"
      },
      "return": [
        "CUR_REV"
      ]
    },
    {
      "command": "GetUnityProjectVersion",
      "params": {
        "projectsettings_path": "#common.PROJECT_PATH#/ProjectSettings/ProjectSettings.asset"
      },
      "return": [
        "PROJ_VERSION"
      ]
    }
  ]
}
```

---

### ㅁ 경력 정리


회사 : 넷마블 에프엔씨
근무 기간: 2022.06~ 재직 중
직종: devops 엔지니어

\# 2024-01-01 이후로 서버 개발자 + devops 엔지니어

\# 주요 업무: CI/CD 구축, 인프라 구축, 모니터링 및 에러 관리, 새로운 Tool R&D

- 2022.06<br>
업무 인수인계

- 2022.07<br>
CI/CD 구축: project demisreborn addressable asset bundle CI/CD 구축<br>
명령어 개선 및 생성: SVN cleanup 커맨드 생성<br>
업무 인수인계<br>

- 2022.08<br>
CI/CD 구축: project GB Inhouse 환경 구성 및 CI/CD 구축<br>
명령어 개선 및 생성: 기존 SVN Clean 커맨드 개선, 기존 커맨드 개선<br>
인프라: <br>
R&D: 모니터링 시스템 구축(Prometheus + grafana + alert manager) <br>

- 2022.09<br>
CI/CD 구축: Epicgames upload, Unreal Engine 스위칭 빌드 <br>
명령어 개선 및 생성: - <br>
인프라: grafana dashboard 생성<br>
R&D: MAC->WIN 빌드 환경 이관 R&D, Unity engine version upgrade & error 처리 <br>

- 2022.10 <br>
CI/CD 구축: 신규 프로젝트 빌드 CI/CD 구축<br>
명령어 개선 및 생성: Jenkins 설치 스크립트 작성, node exporter os별 설치 스크립트 작성 <br>
인프라: Jenkins 구축<br>
R&D: -<br>

- 2022.11<br>
CI/CD 구축: - <br>
명령어 개선 및 생성: samba 서버 설치 스크립트 작성<br>
인프라: 넷마블 에프엔씨에서 관리하는 장비 전수 조사 및 정리, samba 서버 구축<br>
R&D: -<br>

- 2022.12<br>
CI/CD 구축: - <br>
명령어 개선 및 생성: - <br>
인프라: Prometheus SSL + DNS 적용 <br>
R&D: Terraform + Terraformer R&D, Unreal engine 빌드 단축을 위한 증분 거래 등 R&D, window OS chocolate R&D, SVN 마이그레이션 R&D<br>

- 2023.01<br>
CI/CD 구축: -<br>
명령어 개선 및 생성: -<br>
인프라: 모든 프로젝트 Jenkins 이관 및 최신화 <br>
R&D: gitlab stmp R&D, 커맨드 관리 버전 툴 SVN -> gitlab으로 이관 R&D, Centos 7.6 -> rocky linux 8 마이그레이션 R&D, centos 7.6 -> centos 7.9 마이그레이션 R&D<br>

- 2023.02 <br>
CI/CD 구축: - <br>
명령어 개선 및 생성: mercurial 서버 생성 스크립트<br>
인프라: projectGCW SVN -> gitlab 이관, mercurial 서버 생성, 최신버전(13.9) gitlab smtp 변경에 따른 설정 수정<br>
R&D: gitlab backup + restore R&D, Unreal 증분빌드 개선 R&D, freestyle -> pipeline Jenkins R&D<br>

- 2023.03<br>
CI/CD구축: Project Origin 빌드 CI/CD 구축<br>
명령어 개선 및 생성: -<br>
인프라: 모든 프로젝트 빌드 데이터 데이터 수집 및 통계(각 머신 -> GCP big query -> grafana), Proeject Origin 브랜치 생성<br>
R&D: <br>

- 2023.04<br>
CI/CD구축: resource hacker CI/CD 구축<br>
명령어 개선 및 생성: -<br>
인프라: 빌드 완료 후 메시지 보내는 거 개선<br>
R&D: resource hacker R&D, mac OS launchd R&D<br>

- 2023.05<br>
CI/CD 구축: Prometheus 설정 CI/CD 구축<br>
명령어 개선 및 생성: -<br>
인프라: SSL 인증서 교체, grafana 데이터 보관 주기 및 경로 변경, 모든 프로젝트 jenkins pipeline CI/CD 이관 작업, GPU node exporter add & grafana dashboard 구축<br>
R&D: 관리하는 PC들 네트워크 구성도 구현 R&D, gitlab-runner 경로 변경 R&D<br>

- 2023.06<br>
CI/CD 구축: Jenkins 빌드잡 auth 동기화 CI/CD 구축 <br>
명령어 개선 및 생성: SSL 검증 커맨드 생성<br>
인프라: -<br>
R&D: perforce R&D<br>

- 2023.07<br>
CI/CD 구축: android gpg 빌드 CI/CD 구축, crashsight UE5 CI/CD 구축<br>
명령어 개선 및 생성: <br>
인프라: crashsight UE5 환경구성<br>
R&D: Jenkins pipeline 마스킹 R&D, Ansible R&D, windows OS visual SVN R&D<br>

- 2023.08<br>
CI/CD 구축: - <br>
명령어 개선 및 생성: -<br>
인프라: 시놀로지 서버 perforce 구축 및 node exporter 추가, docker 경로 변경<br>
R&D: windos os 서비스 관리 R&D, dvc R&D<br>

- 2023.09<br>
CI/CD 구축: origin 빌드 CI/CD 구축 <br>
명령어 개선 및 생성: -<br>
인프라: incredibuild upgrade 및 사이드이펙트 처리, 프로젝트 origin 머신 생성 및 환경구성, BUILD FRAM 구축, 신규 프로젝트 SVN 서버 구축, helix swarm 서버 구축<br>
R&D: perforce의 helix swarm R&D, Jenkins ldap 연동 에러 처리<br>


- 2023.10<br>
CI/CD 구축: perforce와 SVN sync, 프로젝트 origin 에디터, hlod빌드 CI/CD 구축<br>
명령어 개선 및 생성: -<br>
인프라: 신규 프로젝트 Jenkins 서버 구축<br>
R&D: perforce submit template 구현, perforce submit 시 규칙 강제 trigger 생성, perforce 이전 submit 내역 볼 수 있게 trigger 구현, Prometheus alert 개선, 각종 perforce 에러 및 사용법 R&D 및 공유 + 해결, 그라파나 시놀리지 대시보드 개선, Jenkins pipeline 로직 개선 및 컨플릭트 에러 해결<br>

- 2023.11<br>
CI/CD 구축: perforce-svn sync시 csv 파일 excel 변환 처리 <br>
명령어 개선 및 생성: -<br>
인프라: 대외비 프로젝트 mac 빌드 환경 구성 및 에러 처리, 대외비 프로젝트 cdn 구성 ,Demis Re born 프로젝트 최초 윈도우 빌드 환경 구성<br>
R&D: - <br>

- 2023.12<br>
팀 프로젝트 DemisReborn 으로 이동 및 서버 개발자로 직종 변경<br>
다른 팀에 기존 업무 인수인계<br>

- 2024.01<br>
기존 운영 중인 서버 docker container 로 이관 및 CI/CD 구축

- 2024.02<br>
java spring 기반의 서버 분석 진행중







ㅁ # 지원하는 프로젝트 프로젝트

- ORIGIN
- Grand Cross: Age of Titans
- OverPrime
- DemisReborn
- 아스달
- GoldenBros
- 요괴 듀얼
- 대외비 프로젝트 등

업무: CI/CD 구축, 인프라 및 환경 구축, 모니터링, 새로운 툴 R&D, 모니터링, 에러 해결

---

회사: 한싹<br>
근무 기간: 2021.01~2022.05.<br>
직종: 기술지원 <br>

KT 과금 시스템 솔루션, 비밀번호 변경 솔루션 유지보수 및 설치

인천공항 비밀번호 변경 솔루션설치<br>
2021-05-01 ~ 2022-01-01<br>

- 서버 설정
- 솔루션 설치
- 솔루션 오류 찾기 및 해결, 패치
- 엔지니어용 설치 스크립트 제작
- 엔지니어용 설치 절차 및 오류 종류별 처리 방법 PPT 제작
- 비밀번호 변경 솔루션에 맞는 리눅스 보안 취약점 점검 스크립트 제작
- 사용자들 솔루션 교육
- 준공 서류, 검사 및 각종 필요 서류 제작
- 교육 자료 제작
- OS별 변경 테스트 및 설치, 설정 스크립트 제작

안산시 BTL 사업 비밀번호 변경 솔루션 설치<br>
2021-10-01 ~ 2022-03-01<br>
- 서버 설정
- 솔루션 설치
- CCTV 7 천대 정보 수집, 스위치 3천대 정보 수집 및 솔루션 적용
- 오류 수집 및 해결
- 문서 작성


