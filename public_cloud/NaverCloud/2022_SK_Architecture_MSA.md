지난번 대략적인 소개에 이어 현재 AWS에서 서비스 운영중인 환경 및 정책에 대해서 간략히 소개하고자 한다.

그전에 현재 서비스의 조건에 대해서 정보를 드리면 아래와 같다.

대국민 서비스 (Any Open 필요)
개인정보 처리 시스템 (망분리 필요)
 

위 조건을 바탕으로 아래와 같은 Architecture가 결정 되었다.



![image](https://user-images.githubusercontent.com/62640332/156129719-8a9bc405-1942-4af2-a142-8fa14a4a7254.png)

#### VPC / Subnet / EKS Cluster 정책

- 개인정보를 처리하는 시스템이라면 SKT 보안 규정상 망분리가 필요하다. 우리가 처음 개발하면서 놓친 부분인데, 회사로 부터 Private IP 대역을 할당 받고, 그 대역 기반으로 VPC 생성 및 Subnetting을 해야 했는데 경험 부족으로 놓친부분이기도 했다.

Availability Zone은 3개를 할당하여 가용성을 높히기


![image](https://user-images.githubusercontent.com/62640332/156129905-482a19c5-4e11-487c-b175-3ab80a63368f.png)


- Public/Private Subnet 각각 3개 생성하여 Security 강화
  - Public Subnet에는 NAT Gateway, Bastion Server(Kubectl 전용)만 설치
  - EKS Cluster Node는 Private Subnet에 설치하여 모든 서비스(WEB/WAS)를 Private zone에서 운영. EKS node는 SSH 접속이 필요 없음.


![image](https://user-images.githubusercontent.com/62640332/156129986-5aef2260-077f-4267-a215-21fe3fbc1bc2.png)


Load Balancer 분리
Application 전용 ALB(Application Load Balancer)를 설치하여 Any open 적용하고, 서비스만 담당

Management  전용 Classic Load Balancer를 설치하여 SSH 및 ArgoCD CI/CD등 Management 전용으로 운영하고, Security Group은 회사 사내 IP 대역만 연결.


![image](https://user-images.githubusercontent.com/62640332/156130018-78ac1f23-50ea-40fb-bedc-e5ddfd900380.png)


#### 서비스 도메인 DNS Setting
현재 모바일 지갑 서비스는 아래 기본 Domain에 다양한 Sub Domain을 사용하고 있다.  하지만 모든 Sub Domain은 하나의 L/B cname만 사용하고 있고, 실제 Routing은 Istio Gateway에서 처리를 한다.

![image](https://user-images.githubusercontent.com/62640332/156130145-bb2be175-e906-4bc4-b095-5d6ba5e02055.png)

![image](https://user-images.githubusercontent.com/62640332/156130164-81cce28d-125f-452b-aeb1-e96945394878.png)

실제 서비스에 적용된 Istio ingress 내부 routing을 위한 patch-istio-ingress.yaml의 Example (실제 domain name만 가상으로 변경)

Istio ingress gateway의 설정에 대한 부분은 다음 시간에 자세히 다룰예정.

```
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: mwp-gateway
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - www.domain.co.kr
    - admin.domain.co.kr
    - portal.domain.co.kr

---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: platform-vs
spec:
  hosts:
  - www.domain.co.kr
---
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: admin-vs
spec:
  hosts:
  - admin.domain.co.kr
---
```
domain 이후 uri를 match하여 해당 Service로 routing 기능도 가능하다. 이를 통해 back-end와 front-end를 동일한 URL로 철하고 있다.

```
##################################################################################################
# Platform virtual service
##################################################################################################
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: platform-vs
spec:
  hosts:
  - "*"
  gateways:
  - mwp-gateway
  http:
  # Cloud Agent Datastore
  - match:
      - uri:
          prefix: /agent/ds/
      - uri:
          prefix: /agent/ds
    rewrite:
      uri: /
    route:
```
Cluster 내부의 network flow를 최종 정리 하면, admin 서비스 요청이 들어왔을때 아래 그림과 같다

1. DNS에서 ALB 연결
2. ALB에서 Istio ingress gateway로 요청 전달
3. Istio gateway에서 admin virtual service로 요청 전달
4. Virtual service에서 해당 service로 요청 전달
5. 실제 pod에서 서비스 동작


![image](https://user-images.githubusercontent.com/62640332/156130294-6afed5ec-782b-4d6d-96b4-b4baf969c4ae.png)


#### Summary

- Private Subnet에 EKS node를 모두 설치하여 보안 강화. 심지어 Web service도 public이 아닌 private subnet에서 관리
- Bastion을 통해 kubectl으로 kubernetes 설정을 진행하고, SSH 사용은 안함
- 모든 domain은 하나의 cname으로 mapping 관리하고, Istio ingress에서 나머지 서비스 routing 관리하여 DNS 관리에 대한 부담 최소화






출처] https://devocean.sk.com/blog/techBoardDetail.do?page=&query=&ID=163545&searchData=Kubernetes&idList=%5B163605%2C+163545%2C+163551%2C+163544%2C+163530%2C+163522%2C+163425%2C+163485%2C+163423%2C+163434%2C+163431%2C+163430%2C+163429%2C+163379%2C+163365%2C+163350%2C+163349%2C+163261%2C+163153%2C+163157%2C+163006%2C+163007%2C+163008%2C+162970%2C+162905%2C+162904%2C+162862%2C+162812%2C+162784%2C+162778%2C+162772%2C+162739%2C+162721%2C+162594%2C+161176%2C+160793%2C+159813%2C+159723%2C+159334%2C+159338%2C+159002%2C+158965%5D&subIndex=