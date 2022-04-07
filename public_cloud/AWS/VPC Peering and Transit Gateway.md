
### VPC Peering

: VPC 피어링 연결은 프라이빗 IPv4 주소 또는 IPv6 주소를 사용하여 두 VPC 간에 트래픽을 라우팅할 수 있도록 하기 위한 두 VPC 사이의 네트워킹 연결입니다. 

동일한 네트워크에 속하는 경우와 같이 VPC의 인스턴스가 서로 통신할 수 있습니다. 

사용자의 자체 VPC 또는 다른 AWS 계정의 VPC와 VPC 피어링 연결을 만들 수 있습니다.

VPC는 다른 리전에 있을 수 있습니다(리전간 VPC 피어링 연결이라고도 함).

<br>

### Transit Gateway

: AWS Transit Gateway는 고객이 자신의 Amazon Virtual Private Cloud(VPC)와 온프레미스 네트워크를 단일 게이트웨이에 연결할 수 있도록 지원해 주는 서비스입니다.

AWS에서 실행하는 워크로드가 점차 늘어나고 있으므로 이러한 증가에 발맞추기 위해 여러 계정과 Amazon VPC 전반에서 네트워크를 확장할 수 있어야 합니다. 

현재는 피어링을 사용하여 Amazon VPC의 페어를 연결할 수 있습니다. 

그러나, 지점 간 연결 정책을 중앙에서 관리하는 기능 없이 여러 Amazon VPC 전반의 지점 간 연결을 관리하면 운영 비용이 많이 들고 번거로울 수 있습니다. 

온프레미스 연결의 경우 AWS VPN을 각 Amazon VPC에 따로 연결해야 합니다. 

이 솔루션은 구축하는 데 시간이 오래 걸리며 여러 VPC가 수백 개로 늘어나면 관리하기가 힘들어질 수 있습니다.

<br>

### VPC Peering 과 Transit Gateway

서로 VPC 를 연결한다는 공톰점이 있지만 VPC 를 연결시에 생기는 구조에 차이가 있습니다.

예를 들면 3개의 VPC 와 1개의 On-Premises 를 모두 연결할 때 VPC Peering 과 Transit Gateway 의 차이는 아래의 그림과 같습니다. (On-Premises 는 VPN Connection 을 사용하여 연결 하였습니다.)

![image](https://user-images.githubusercontent.com/62640332/161773680-e944c22a-4a97-4398-80c2-d32c0c21e730.png)

VPC Peering 의 경우 VPC 와 VPC 즉 2개의 VPC 끼리만을 연결하기 때문에 여러 VPC 를 사용하게 되면 그에 따라 VPC Peering 의 개수도 증가하게 됩니다.

또한, On-Premises 를 모든 VPC 에 연결하기 위해서는 모든 VPC 에 VPN Connection 을 해주어야 합니다.

하지만, Transit Gateway 는 여러 VPC 를 한번에 연결할 수 있게 합니다. 또한 VPN Connection 도 Transit Gateway 에 연결 함으로써 VPN Connection 의 개수가 줄어들 수 있습니다. 

이러한 특성 때문에 Transit Gateway 를 사용하게 되면 구조를 좀 더 단순한 형태로 만들 수 있습니다.


<br>

### VPC Peering 과 Transit Gateway 의 공통점

- 리전간의 VPC 연결
- 다른 AWS 계정의 VPC 과의 연결

VPC Peering 은 모든 상용 리전(중국 제외) Transit Gateway 은 2020년 4월 부터 이하의 리전에서 리전간 VPC 연결을 지원하게 되었습니다.

리전간 VPC 피어링, 서울 리전 출시

다른 계정간 VPC 연결은 VPC Peering, Transit Gateway 모두 지원합니다.


<br>

### VPC Peering 과 Transit Gateway 의 차이점


||VPC Peering|Transit Gateway|
|--|--|--|
|대역폭|제한 없음| 50Gbps|

트래픽은 프라이빗 IP 공간 안에서 유지됩니다. 모든 리전 간 트래픽은 암호화되며 단일 장애 지점 또는 대역폭 제한이 없습니다.

<br>

### 최대 구성 가능 개수

- VPC 당 최대 VPC Peering 구성 가능한 개수는 125개입니다.

- Transit Gateway 당 VPC 연결(Attachment)은 5000개 까지 가능하게 됩니다.


Transit Gateway 을 사용하는게 좀 더 단순한 구조를 만들 수 있습니다, 하지만 VPC Peering 을 사용하는 경우보다 비용이 더 나오기 때문에, 

대역폭의 제한이 문제가 될 수 있거나, 소수의 VPC 를 운용할 경우에는 VPC Peering 이 더 유용할 수 있다고 생각합니다.

VPC Peering 과 Transit Gateway 를 각 특성에 맞게 부분적으로 사용하는 것도 방법 중 하나라고 생각합니다.

On-Premises 를 다수의 VPC 에 연결해야 하는 경우 Transit Gateway 를 사용한다면 VPN Connection 을 줄여서 비용을 줄일 수 있다고 생각했지만 

Transit Gateway VPC 연결 비용이 VPN Connection 비용보다 비싸기 때문에 Transit Gateway 가 비용이 더 나오게 됩니다.



---

<출처>

https://dev.classmethod.jp/articles/different-from-vpc-peering-and-transit-gateway/