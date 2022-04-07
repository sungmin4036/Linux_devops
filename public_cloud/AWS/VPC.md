### IP(Internet Protocol)

IP는 크게 '공인 IP'와 '사설 IP'로 나누어 볼 수 있습니다. 

공인 IP는 인터넷망에서 한 단말에서 한 단말로 접속할 수 있는 주소로 인터넷 망 내부에서 고유한 존재입니다.

즉 공인 IP는 어느 한 군데에서 사용 중이라면 다른 곳에서 사용할 수 없습니다.

### VPC(Virtual Private Cloud)

: Amazon Virtual Private Cloud(Amazon VPC)에서는 사용자가 정의한 가상 네트워크로 AWS 리소스를 시작할 수 있습니다. 

이 가상 네트워크는 AWS의 확장 가능한 인프라를 사용한다는 이점과 함께 고객의 자체 데이터 센터에서 운영하는 기존 네트워크와 매우 유사합니다.

<br>

Private Network를 활용하여 네트워크망을 구성하고 내부에 각종 Resource를 탑재할 수 있는 서비스를 VPC라 합니다. 

VPC를 생성하는 경우, 다음과 같이 /16RFC 1918:규격에 따라 프라이빗(비공개적으로 라우팅 가능) IPv4 주소 범위에 속하는 CIDR 블록( 또는 이하)을 지정하는 것이 좋습니다.

AWS VPC는 On-premise(전통적인 인프라)와 동일한 대역의 사설 IP를 사용할 수 있으며 모든 AWS 계정은 위의 사설 IP 대역을 사용할 수 있습니다.

다만 다른 점이 있다면 원래 규정된 사설 IP 범위와는 다르게 /16 ~ /28비트의 서브넷 마스크만을 허용한다는 것이 있습니다.

즉 VPC를 생성할 수 있는 가장 큰 대역은 /16이며, 가장 작은 대역은 /28입니다.

VPC 내에서 다시 크기를 쪼개서 알맞게 사용하게 되는데 VPC를 쪼갠 조각들을 Subnet이라고 합니다.

그리고 하나의 Subnet은 하나의 AZ(Availability Zone)에 할당됩니다. 

서울 Region의 경우 AZ가 3개인데, 각기 다른 AZ를 가진 서브넷을 최소 3개 이상 만들 수 있다는 뜻이 됩니다.

<br>
<br>

각 서브넷은 동일한 네트워크 내에서 통신시 Routing Table을 필요로 하지 않습니다. 그리고 AWS가 관리용으로 사용하는 IP도 존재합니다.


<br>
<br>

```
Routing Table

라우팅 테이블(영어: routing table)은 컴퓨터 네트워크에서 목적지 주소를 목적지에 도달하기 위한 네트워크 노선으로 변환시키는 목적으로 사용된다.  

각 라우터의 라우팅 테이블은 모든 목적지 정보에 대해 해당 목적지에 도달하기 위해서 거쳐야 할 다음 라우터의 정보를 가지고 있다. 
```


한 Subnet이 다른 Subnet으로 가기 위해서는 'Routing'이 필요합니다. 각각의 Subnet은 서로 다른 네트워크 영역이기 때문이지요. 

그렇다면 VPC를 다루기 위해서는 Routing을 어떻게 설정해야 할까요?

모든 라우팅 테이블에는 VPC 내부 통신을 위한 로컬 라우팅이 포함되어 있습니다. 

이 라우팅은 기본적으로 모든 라우팅 테이블에 추가됩니다. VPC에 하나 이상의 IPv4 CIDR블록이 연결되어 있는 경우, 라우팅 테이블에 각 IPv4 CIDR 블록의 로컬 경로가 포함됩니다. 

이러한 라우팅을 수정하거나 삭제할 수 없습니다.

VPC 내부(모든 Sunbet)에 대해서는 Routing이 자동으로 생성됩니다. 즉 별도의 설정 없이 한 Subnet에서 다른 Subnet으로 통신이 가능하다는 뜻입니다. 

이는 사용자가 설정할 필요가 없고, 보이지도 않는 암시적 라우터인 'VPC Router'를 통해 가능합니다. 

Subnet 내의 모든 Resource는 자신이 소속된 Subnet이 아닌 다른 곳으로 향하고자 할 때 VPC Router를 거쳐야 합니다.

라우팅 테이블은 Local Routing 뿐만 아니라 Subnet에 소속된 Resource(EC2 등)가 외부 인터넷망으로 나갈 수 있는 Routing을 가질 수 있습니다. 이를 Internet Gateway라고 합니다.

```
Internet Gateway
인터넷 게이트웨이는 수평 확장되고 가용성이 높은 중복 VPC 구성 요소로, VPC의 인스턴스와 인터넷 간에 통신할 수 있게 해줍니다. 

인터넷 게이트웨이에는 인터넷 라우팅 가능 트래픽에 대한 VPC 라우팅 테이블에 대상을 제공하고, 

퍼블릭 IPv4 주소가 할당된 인스턴스에 대해 NAT(네트워크 주소 변환)를 수행하는 두 가지 목적이 있습니다.
```

Internet Gateway만 생성해두면 외부 Internet과 통신할 수 있는 것일까요? 아닙니다!  

VPC에서 Resource(EC2 등)가 외부 인터넷과 통신하고자 할 경우 갖추어야 할 조건은 다음과 같습니다.


1. Internet 통신하고자 하는 Resource가 공인 IP를 보유할 것

2. Resource가 소속된 Subnet의 Routing Table에 '0.0.0.0/0' 목적지로 갖는 Routing 'Internet Gateway'이 있을 것

3. Network ACL과 Security Group 규칙에서 허용할 것

AWS에서는 공인 인터넷과 통신 가능한 Subnet을 'Public Subnet', 공인 인터넷이 차단된 사설 IP만 할당된 Subnet을 'Private Subnet'이라 부릅니다. 

<br>
<br>

```
NAT(Network Address Translation)
네트워크 주소 변환(영어: network address translation, 줄여서 NAT)은 컴퓨터 네트워킹에서 쓰이는 용어로서, 
IP패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술을 말한다.
NAT를 이용하는 이유는 대개 사설 네트워크(Private Network)에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위함이다.
NAT가 호스트 간의 통신에 있어서 복잡성을 증가시킬 수 있으므로 네트워크 성능에 영향을 줄 수 있는 것은 당연하다.

NAT를 이용하는 이유는 대개 사설 네트워크(Private Network)에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하기 위함이다. 
```


IP 주소에는 Public IP(공인 IP)와 Private IP(사설 IP)가 있습니다. 

IP를 굳이 두 종류로 나눈 이유를 간단히 말씀드리면 IPv4 주소의 낭비를 막고 공인 인터넷을 굳이 사용하지 않아도 되는 단말들에게 어느 망이든 중복 사용 가능한 IP를 주기 위함이지요.(공인망에서는 공인 IP만이 유통됩니다.) 

그런데 사설 IP를 할당받은 단말이 인터넷을 사용해야 한다면 단말의 IP는 어떻게 변화하는 것일까요?

사설 IP를 Source Address로 유지한 채 인터넷으로 나아가는 것일까요? 아닙니다. 공인인터넷망과 제 컴퓨터 사이에는 제 컴퓨터에게 IP를 할당해주는 공유기가 있습니다. 

그리고 공유기는 공인 IP를 보유하며 인터넷과 맞닿아있지요. 

Private IP(172.30.1.36)뿐인 제 컴퓨터가 외부 서비스(8.8.8.8)을 사용하고자 공유기로 나아가면 공유기는 Private IP(172.30.1.36)뿐인 제 컴퓨터의 IP를,

즉 Network Address를 공유기 자신의 공인 IP로 변환(Translation)합니다. 즉 공유기를 지나 외부 인터넷으로 나아갈 때에는 공유기의 공인 IP를 가지고 원하는 Destination Address로 향하는 것이지요.

이것이 NAT입니다.(다양한 용도가 있습니다. 지금은 극히 일부를 설명한 것입니다.)

![image](https://user-images.githubusercontent.com/62640332/161787826-595a89ab-797f-4c9d-920f-1a284721f30a.png)

외부 인터넷을 차단하고 내부에서만 사용하기 위해 만든 Subnet이지요. Database Service인 RDS를 넣어두려고 합니다. 

그리고 RDS는 외부에서의 접속을 철저히 차단하고 싶습니다. 그런데 RDS가 외부 인터넷을 통해 업데이트를 해야 할 일이 생긴다면 어떻게 해야할까요? 

공인 IP를 생성해서 Internet Gateway를 연결해줘야할까요? 그러면 외부에서도 침입이 가능해질텐데 보안이 걱정됩니다. 

이러한 경우에 사용하는 것이 `NAT Gateway`입니다. Internet 접속이 가능한 Public Subnet에 NAT Gateway를 생성해두고 Private Subnet이 외부 인터넷으로 나아갈 경우에만 사용하도록 라우팅을 추가해주는 것이지요.

위 NAT에서 설명한 것처럼 Private Subnet의 서비스들은 외부 인터넷으로 나갈 때 공인 IP로 NAT되어 인터넷망을 헤엄치게 됩니다. 

그러나 NAT Gateway는 내부에서 외부로의 접속만 가능하며 외부에서 NAT Gateway를 이용하여 접속하는 것은 불가능합니다.

```
EC2(10.0.3.x/24, Private Subnet) - > NAT Gateway - > Internet Gateway - > 외부 인터넷
```

<br>
<br>

VPC와 Subnet은 AWS 계정의 사설 네트워크를 구성하고 각종 서비스들을 탑재할 수 있다고 언급하였습니다. 

서비스들의 트래픽이 지나다는 곳인 만큼 트래픽에 대한 제어와 통제, 확인이 반드시 필요합니다.

- 서비스들과 트래픽의 흐름을 기록하는 서비스 :  Security Group, Network ACL, VPC FLow Logs

![image](https://user-images.githubusercontent.com/62640332/161788818-211eeadb-b497-4d3a-9ca0-dca99a1f0024.png)

Route table 아래 자물쇠가 채워진 첫 번째 상자는 Network ACL입니다. 

Subnet을 오가는 트래픽을 제어하는 역할을 합니다. 그렇기에 그림에서 Subnet 상자 위에 위치한 것을 볼 수 있습니다. 

두 번째 상자는 Subnet 내부에 위치한 Security Group입니다. Subnet 내 인스턴스의 트래픽을 제어할 수 있습니다. 

왼쪽 Subnet과 오른쪽 Subnet의 Security Group이 모양이 조금 다릅니다. 다수의 인스턴스가 하나의 Securiy Group을 쓰거나, 각자의 Security group을 쓸 수 있음을 의미합니다.

 
세 번째로 그림에는 나와있지 않지만 VPC Flow Logs입니다. VPC 내에 있는 Resource들은 Elastic Network Interface(ENI), 네트워크 인터페이스를 가지는데요.

Network Interface에 VPC Flow Logs를 설정하면 Elastic Network Interface(ENI)를 지나다니는 IP 트래픽을 수집할 수 있습니다. 


```
Security Group(보안그룹)
보안 그룹은 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽 역할을 합니다. 

VPC에서 인스턴스를 시작할 때 최대 5개의 보안 그룹에 인스턴스를 할당할 수 있습니다.

보안 그룹은 서브넷 수준이 아니라 인스턴스 수준에서 작동하므로 VPC에 있는 서브넷의 각 인스턴스를 서로 다른 보안 그룹 세트에 할당할 수 있습니다.
```

Security Group은 인스턴스의 가상 방화벽 역할을 합니다. 

여기서 말하는 인스턴스는 EC2 인스턴스뿐만 아니라 VPC 내에 탑재될 수 있는 수많은 가상 컴퓨터를 의미합니다. 

그러므로 EC2, RDS, ELB, VPC Interface Endpoint 등도 포함될 수 있습니다. 

다른 말로 표현하면 VPC 내에서 Elastic Network Inteface(ENI)를 갖는 모든 서비스라고 할 수 있습니다. 

정확히 말하면 Security Group은 인스턴스의 ENI에 적용되는 것이기 때문입니다. 

또한 인바운드 규칙(외부 - > 인스턴스)과 아웃바운드 규칙(인스턴스 - > 외부)으로 나뉘며 모든 트래픽은 규칙에 따라 제어됩니다.

ㅁ Security Group은 몇 가지 특징

1. 인바운드 규칙과 아웃바운드 규칙으로 나뉨
2. 허용 규칙만 생성 가능
3. 기본적으로 모든 Security Group의 아웃바운드 규칙은 모든 아웃바운드 트래픽 허용
4. 하나의 인스턴스에는 최대 5개의 Security Group 동시 적용 가능
5. 상태를 저장하여 한 번 인바운드를 통과하는 트래픽은 아웃바운드의 규칙 적용을 받지 않음(Stateful)
6. 상태를 저장하여 한 번 아웃바운드를 통과하는 트래픽은 인바운드의 규칙 적용을 받지 않음(Stateful)

5번과 6번이 Securiy Group의 가장 큰 특징이라고 할 수 있겠습니다. 이를 Stateful이라 합니다. 

사용 중인 Security Group을 보면 HTTP/HTTPS/SSH의 허용은 인바운드에만 적용되어있습니다. 

외부에서 들어오는 트래픽은 아웃바운드의 규칙을 전혀 적용받지 않기 때문에 인바운드에만 허용시켜두면 3개 프로토콜 트래픽의 왕래는 전혀 문제가 없습니다.


```
Network ACL(네트워크 액세스 제어)
네트워크 ACL(액세스 제어 목록)은 1개 이상의 서브넷 내부와 외부의 트래픽을 제어하기 위한 방화벽 역할을 하는 VPC를 위한 선택적 보안 계층입니다.
보안 그룹과 비슷한 규칙으로 네트워크 ACL을 설정하여 VPC에 보안 계층을 더 추가할 수 있습니다.
```

Network ACL은 Subnet의 Access List(접근 제어 목록)을 책임집니다. 

Cisco Network Device를 다루어보셨다면 익숙한 이름일겁니다. Subnet을 오고 가는 모든 트래픽을 제어하는 역할을 합니다. 

Network ACL은 Subnet 단위로 적용되기 때문에 Subnet 내의 모든 트래픽은 Network ACL의 규칙 적용을 받습니다. 

Security Group과 달리 '규칙' 값이 존재하고 허용/거부 항목이 눈에 띕니다. Security Group과 달리 별다른 규칙을 정해두지 않았습니다.


ㅁ Network ACL의 특징
1. 인바운드 규칙과 아웃바운드 규칙으로 나뉨
2. 허용 규칙뿐만 아니라 거부 규칙 생성 가능
3. 규칙에 값이 매겨져 가장 작은 값을 지니는 규칙이 우선적으로 적용됨
4. 하나의 Subnet에는 하나의 Network ACL만이 적용됨
5. 상태를 저장하지 않아 한 번 인바운드를 통과하는 트래픽은 아웃바운드의 규칙 적용을 받음(Stateless)
6. 상태를 저장하지 않아 한 번 아웃바운드를 통과하는 트래픽은 인바운드의 규칙 적용을 받음(Stateless)

Security Group처럼 5번과 6번이 Network ACL의 가장 큰 특징이라고 할 수 있겠습니다. 

Stateless라고 부릅니다. 인바운드 규칙을 적용받아 유입된 패킷이 다시 밖으로 나가기 위해서는 아웃바운드 규칙의 적용을 받아야 합니다. 

또한 모든 규칙이 적용되는 Security Group과 달리 Network ACL은 규칙 순서가 있어 가장 작은 규칙 값을 갖는 규칙이 가장 먼저 적용되는 특징이 존재합니다.

ㅁ 어떻게 사용하는 것이 좋을까?

1. Security Group을 사용해 각 인스턴스에 허용되어야 할 프로토콜과 포트를 각각 지정함(IP는 지정하지 않음)
2. Network ACL을 사용해 모든 트래픽을 허용하되, 차단할 필요가 있는 IP 차단

프로토콜과 포트 설정을 Security Group에 전적으로 맡겨 각 인스턴스에 필요한 규칙을 서비스 특성에 맞춰 설정하고, 

Network ACL은 프로토콜과 포트 설정에 관여하지 않고 트래픽을 허용하되, 악의적 트래픽이 의심되는 IP를 규칙 값을 이용해 우선적으로 차단하여 관리의 효율을 높인다고 생각합니다.

![image](https://user-images.githubusercontent.com/62640332/161790021-b3392806-e6e7-4256-ac57-32b597c9878b.png)

```
VPC Flow Logs

VPC, 서브넷 또는 네트워크 인터페이스에 대한 흐름 로그를 생성할 수 있습니다. 
서브넷이나 VPC에 대한 흐름 로그를 생성할 경우, VPC 또는 서브넷의 각 네트워크 인터페이스가 모니터링됩니다.
```


VPC 내 Resource들은 Elastic Network Interface(ENI)를 갖는다고 위에서 설명하였습니다. 그리고 이 ENI를 통해 트래픽이 흐릅니다. 

VPC Flow Logs는 VPC, Subnet, ENI에 흐르는 IP 트래픽을 수집하는 역할을 하며 NAT Gateway, Internet Gateway에 대한 트래픽 수집도 가능합니다.

VPC Flow Logs 설정은 VPC 단위로 가능합니다. 트래픽을 선별적으로 수집할 수 있어 허용/거부/모든 트래픽을 지정할 수 있고 Cloudwatch Logs 혹은 S3로 전송이 가능합니다. 이 경우에는 요금이 발생할 수 있습니다.

인스턴스에 제대로 트래픽이 들어오지 않는다고 의심되거나 Security Group이 제대로 적용되어있는지를 확인하기 위해 VPC Flow Logs를 사용합니다.

또한 다음과 같은 트래픽은 VPC Flow Logs에 기록되지 않으므로 주의해야 합니다.

1. 인스턴스가 Amazon DNS 서버에 연결할 때 생성한 트래픽
2. Amazon Windows 라이선스 인증을 위해 Windows 인스턴스에서 생성한 트래픽
3. 인스턴스 메타데이터를 위해 169.254.169.254와 주고받는 트래픽
4. Amazon Time Sync Service를 위해 169.254.169.123과 주고받는 트래픽
5. DHCP 트래픽
6. 기본 VPC 라우터의 예약된 IP 주소로 보내는 트래픽

### VPC Endpoing는 왜필요한가?

AWS VPC는 사설 네트워크로 이루어진 사용자 정의 네트워크입니다. VPC 내 EC2, RDS, ELB 등을 탑재하고 ENI(Elastic Network Interface)에 사설 IP 혹은 공인 IP를 부여하여 사용하지요. 

S3, Cloudwatch, Cloudfront, Dynamo DB, API Gateway 등은 어떨까요? VPC 내부에 탑재하여 사용하는 서비스들인가요? 아닙니다. 

AWS의 Region 내에 존재하는 것은 사실이지만 VPC 내부에 존재하지 않고 공인 IP를 가지고 외부에서 접근하는 서비스들입니다. 

VPC 내부의 Resource들이 S3와 같은 서비스를 이용하기 위해서는 외부 인터넷을 통해 도달해야 합니다.

VPC 내부의 Resource들 또한 마찬가지입니다. Internet Gateway 혹은 NAT Gateway가 생성되어있지 않아 외부 인터넷에 접근할 수 없다면 S3와 같은 서비스에 접근할 수 없습니다.

```
EC2(10.0.3.20, Private Subnet) - > NAT Gateway - > Internet Gateway - > 외부 인터넷 - > S3
```

결국 VPC 내부 Resource와 기타 AWS Service Endpoint(EC2 API 호출, S3 접속 등)와 통신 시 외부 인터넷에 공개적으로 연결되며 트래픽이 노출됨을 의미합니다. 

보안상으로 썩 좋지 않은 방법이네요. 이를 해결하기 위한 것이 VPC Endpoint입니다.

```
VPC Endpoint

VPC 엔드포인트를 통해 인터넷 게이트웨이, NAT 디바이스, VPN 연결 또는 AWS Direct Connect 연결을 필요로 하지 않고 
AWS PrivateLink 구동 지원 AWS 서비스 및 VPC 엔드포인트 서비스에 비공개로 연결할 수 있습니다.

VPC의 인스턴스는 서비스의 리소스와 통신하는 데 퍼블릭 IP 주소를 필요로 하지 않습니다. VPC와 기타 서비스 간의 트래픽은 Amazon 네트워크를 벗어나지 않습니다.
```

VPC Endpoint는 VPC 내부 Resource가 VPC 외부 AWS 서비스와 통신할 때 외부 인터넷을 거치지 않고 'AWS 백본 네트워크'를 통해 서비스에 도달할 수 있도록 지원하는 서비스입니다.

그러므로 외부 AWS 서비스와 통신할 때 Public IP를 필요로 하지 않습니다.  

말 그대로 VPC 내부에 Endpoint를 형성한 뒤, 이 Enpdoint를 통해 AWS 외부 서비스에 도달할 수 있도록 합니다. `Interface endpoint`와 `Gateway endpoint`로 나뉩니다. 

![image](https://user-images.githubusercontent.com/62640332/161791003-9b21eb86-3779-4979-83a9-1fdd187fe278.png)

위의 그림은 Interface endpoint를 설명하고 있습니다. 

1개 이상의 Subnet에 Endpoint network interface를 생성한 뒤 사설 IP를 부여합니다. 

Interface endpoint를 생성하기 전(초록색 선)에는 공인 IP를 보유한 Resource만이 Internet Gateway를 통해 Kinesis Streams에 접근할 수 있었지만

Interface endpoint를 생성한 이후(파란색 선)에는 공인 IP 없이도 Subnet 1, Subnet 2의 EC2 인스턴스 모두 Endpoint network interface를 통해 Kinesis Streams에 접근할 수 있습니다.

이 모든 과정은 AWS PrivateLink를 이용하여 이루어집니다.

![image](https://user-images.githubusercontent.com/62640332/161791296-6b56194d-3d56-441a-813e-f951972eb032.png)

위의 그림은 Gateway endpoint를 설명하고 있습니다. 

Endpoint network interface를 생성하는 Interface endpoint와 달리 Gateway endpoint는 Gateway endpoint를 생성한 후, Subnet에서 Routing만을 추가로 생성됩니다. 

자동으로 생성되므로 변경할 수 없습니다. 해당 Routing table 경로를 보고 VPC Endpoint를 통해 S3에 접근 가능합니다. 

S3, Dynamo DB 두 개 서비스만 사용 가능합니다.




---

[출처]

1. https://aws-hyoh.tistory.com/49
2. https://aws-hyoh.tistory.com/53?category=768982
3. https://aws-hyoh.tistory.com/58?category=768982
4. https://aws-hyoh.tistory.com/72?category=768982
5. https://aws-hyoh.tistory.com/73?category=768982
