Argo CD는 쿠버네티스 환경에서 지속적 전달을 통해 서비스를 배포하기 위한 전략을 도와주는 오픈소스 툴킷이다.

Argo CD는 쿠버네티스 클러스터 내부에 Pod 형태로 배포가 되며, 실제 쿠버네티스 클러스터 내부에서의 역할이 정확히 무엇인지는 아래 그림을 참고한다.

![image](https://user-images.githubusercontent.com/62640332/156985111-6bced294-838b-4eba-8975-20da0a84839e.png)

Argo CD는 특정 원격 저장소(GitOps Repository)의 내용을 감지하여 내용의 변경사항(Diff)이 발견되면 이를 사용자에게 알려주고 반영할지 여부를 물어본다.

여기서 우리는 특정 원격 저장소(GitOps Repository) 라는 것이 무엇인지 알아야 하는데, 이 GitOps Repository는 이름 그대로 

운영(Operation)에 관련된 리소스를 Git으로 관리하고 있는 원격 저장소(Repository) 라고 생각하면 된다.

쿠버네티스에서 배포나 서비스 설정을 위해 사용하는 매니페스트 리소스(=yaml 파일)들을 관리하는 원격 저장소라고 보면 된다.

여기서 특정 원격 저장소라고 거창하게 이야기 했지만, 사실상 특별한 것 이 없이 그저 GitHub, Bitbucket, 또는 Gitlab과 같은 원격 리포지토리 안에 쿠버네티스에서 사용할 매니페스트 리소스들만 들어가있는 것 뿐이다.

또한 Argo CD는 자신만의 대시보드를 제공해준다.

이 대시보드에서 Argo CD를 통해 배포된 많은 서비스들을 한눈에 볼 수 있으며, 새로운 어플리케이션의 배포 및 롤백, 관리도 가능하다.


출처] https://medium.com/finda-tech/eks-cluster%EC%97%90-argo-cd-%EB%B0%B0%ED%8F%AC-%EB%B0%8F-%EC%84%B8%ED%8C%85%ED%95%98%EB%8A%94-%EB%B2%95-eec3bef7b69b