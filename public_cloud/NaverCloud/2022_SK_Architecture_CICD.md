현재 2개의 상용플랫폼을 개발/운영하고 있습니다.

블록체인 기반 DID/전자증명 플랫폼 ('21.3월 상용완료)
SKT 통합 Digital Asset 서비스를 위한 모바일지갑 플랫폼 ('22.1월 상용 예정)
MSA, Kubernetes, DevOps, Istio, Datadog, Public Cloud, Blockchain, NFT, GitHub Action, Gitops, ArgoCD...등등

요즘 힙한 기술들을 적용하여 개발운영(DevOps) 하면서, 그동안 느낀점과 공유하고 싶은 이야기를 적어 봅니다.

각 기술에 대한 전문적인 글이 아닌 서비스를 DevOps 하면서 느낀점을 블로그 형식으로 편하게 작성하였습니다.

 

MSA(Microservice Architecture) 도입

![image](https://user-images.githubusercontent.com/62640332/156127968-0a1ac7cc-c784-4e76-ae0b-db99ccb0f3cb.png)



내재화 개발 기획 당시 개발 효율을 위해 다양한 Open Source를 활용해야 했고, 첨부 그림과 같이 JAVA, Python, Node, Vue, Golang등 다양한 Program Langauge와 Framework이 사용될 수 밖에 없었다. 그러다 보니 Monolithic Architecture는 선택할 수 없었고, 반강제 MSA 방식을 도입하게 되었다. (현재 위 그림보다 microservice가 더 증가하였다)

 

#### Kubernetes 운영 환경 

MSA를 제대로 운영하기 위해서는 Kubernetes가 유일한 옵션이었다. 다행히 Public Cloud 사용 권장으로 설치가 복잡한 Kubernetes를 Managed Service로 쉽게 사용할 수 있게되었다. 아래와 같이 관리도구 포함 59개의 서비스를 직접 관리한 다는 것은 거의 불가능한 일이다.

![image](https://user-images.githubusercontent.com/62640332/156128147-fc40a9d7-15f6-4c60-ba52-57c7df1643dd.png)

현재는 Kubernetes의 장점을 최대한 활용하기 위해 현재 각 서비스/Pod마다 Load Test를 실시하고 있으며, 이를 통한 가장 효율적인 구성으로 세팅되어 있으며, Scale-out 정책도 수립되었다. 현재 운영은 4개의 node만으로 10개 이상의 서비스들이 동작이 가능하다. 이부분은 향후 Kubernetes 상세 운영 과정을 통해서 설명할 예정

 
#### GitHub Actions를 통한 Repository 관리 및 Build 효율화

이제 협업을 위한 Repository를 결정해야 한다. Repo 결정은 결국 Build 정책 및 CI/CD 정책과 모두 연결된다. TDE에서 제공하는 Bitbucket/Jenkins 조합과, GitHub/GitHub Actions 조합 두가지 중에 고민을 했지만 지금까지 사용해보지 않은 GitHub으로 결정했다.


![image](https://user-images.githubusercontent.com/62640332/156128238-c92a4150-e36f-4a5d-a1f2-effa9c24c92e.png)


GitHub Action의 적용을 위해 모든 Repository 마다 workflow를 작성하고, 우리의 정책은 아래와 같이 정의했다.

![image](https://user-images.githubusercontent.com/62640332/156128345-0ad38745-8239-477d-a1fa-b81cf98c892b.png)

![image](https://user-images.githubusercontent.com/62640332/156128388-2fac0422-1939-4a1f-b282-d52e9b0b3f49.png)

간단하게 요약하면 Slack으로 Start 전송 ⇒ Code Checkout ⇒ Build후 ECR 등록 ⇒ Slack으로 완료 알림 ⇒ Gitops를 위한 config에 container tag 등록 ⇒ Bitbucket에 Code Backup용 Push ⇒ [옵션] Postman API Doc 등록....

오늘은 전체적인 Flow만 설명을 하고, 각 단계별 상세한 내용은 다음에 설명을 하기로 한다. GitHub Actions를 통해 JAVA, Python, Node, Vue, Kotlin, Swift등 모든 언어에 대해서 개발 환경에 영향을 받지 않고, 효율적인 Build가 가능해졌다.

Github Actions로 Build의 효율화를 얻었지만, 운영을 하다보니 무료 Plan의 한계에 도달하기도 했다. 한달에 2000분 무료 Build가 가능하고, 초기에는 충분할 것으로 예상했지만, 서비스가 늘어나고 신규 서비스 출시가 가까울 수록 사용량이 급증하면서 결국 오늘 90%가 넘었다는 경고 메일을 받기로 했다. 하지만 우리는 답을 찾을 것이다. 늘 그랬듯이.....이또한 해결책은 다음번 글에서 설명할 예정.

 
#### Gitops 1.0 도입과 ArgoCD CI/CD

이제 build가 완료된 container의 CI/CD 즉 배포 및 관리를 결정해야 한다. 이미 Kubernetes 사용을 결정한 이상 ArgoCD를 사용할 수 밖에 없다. 현재 2개의 서비스 플랫폼에서 Dev/Prd 각각 총 4개 Cluster 환경이 운영되고 있다.

그리고 각 Cluster에는 최소 10개 이상의 microservice들이 배포되어 있고, HA를 위한 이중화 Pod 배포까지 감안하면 수십개의 pod가 동작하고 있다.

![image](https://user-images.githubusercontent.com/62640332/156128626-a4421f9d-da13-4677-9610-ade5645f2b6b.png)

![image](https://user-images.githubusercontent.com/62640332/156128669-593470a0-ba2c-47a3-9383-68479ceff6d8.png)

배포는 ArgoCD를 사용하지만, 제일 중요한 배포 전략이 필요하였다. 그래서 현존하는 가장 Hip한 Best Practice를 연구하였고, Gitops 1.0을 정책을 따르기로 했다.


참고 : https://velog.io/@wlgns5376/GitOps-ArgoCD%EC%99%80-Kustomize%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%B4-kubernetes%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0

즉 Kustomize를 활용하여 Pull 방식으로 자동 배포하는 방식이다.

![image](https://user-images.githubusercontent.com/62640332/156128841-9c0d2273-c68a-44bb-8950-b5ca97528c1d.png)

이렇게 Configuration을 관리할 수 있는 Repository에 yaml file에 update된 container tag를 등록하고, ArgoCD는 주기적으로 Check하여 자동 배포하는 방식이다.

물론 개발계와 운영계 각각 배포 정책이 필요하고, 심지어 각 Microservice들간 배포 전략도 필요하다. 이는 다음번 배포 전략 부분에서 상세히 다룰 예정이다.

현재 Gitops1.0도 완벽하진 않다. 그래서 현재 Global 전문가들이 Gitops 2.0이 준비되고 있고, 우리도 참고하여 향후 배포 전략에 반영할 예정이다.

 

#### Istio 도입
MSA를 도입하면서 자연스럽게 고민하게 되는 부분이다. 즉 Serive Mesh간 통신 처리를 고민할 수 밖에 없는데, Kubernetes의 기본 ingress gateway는 한계가 명확하다. Kubernetes Ingress는 외부 트래픽에 대한 단일 입구를 제공하는 장점이 있지만, 그에 따른여러 단점이 존재한다. 가장 명확한 건 하나의 서비스에 부하가 걸리면 나머지 통신도 모두 영향을 받을 수 밖에 없다. 이를 해결하기 위해 Envoy Proxy를 도입한 Istio를 Gateway에 도입하게 된다. 그 외 Virtual Service 및 outbound를 관리하는 egress 등 정책 관리부분이 존재한다. 이또한 우리의 플랫폼 상세 운영/정책에 대한 글 작성하면서 update 할 예정이다.

![image](https://user-images.githubusercontent.com/62640332/156128897-ad458fc6-c265-40df-aa39-c2d7bdd79244.png)


#### Monitoring : Datadog으로 통합
DevOps에서 가장 중요한 Monitoring 부분이다. 초기에는 Elasticsearch를 활용한 log와 APM, 그리고 Grafana, Prometheus, Loki등 다양한 Open Source를 활용하였다.

하지만 올 가을 회사에서 Datadog이 공식 제공되면서 모든 고민이 한번에 해결되었다. Log, APM, Kubernetes service monitoring등등...Datadog 세팅도 다음번 상세 내용으로 설명 예정이다.


![image](https://user-images.githubusercontent.com/62640332/156128934-0a340e31-ffeb-41d6-9a4d-85b88d8b512e.png)

![image](https://user-images.githubusercontent.com/62640332/156128957-fee2e32d-ca6b-4bb4-a732-53fbf428638a.png)


출처] https://devocean.sk.com/blog/techBoardDetail.do?ID=163544#none