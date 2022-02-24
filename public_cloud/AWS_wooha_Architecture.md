![image](https://user-images.githubusercontent.com/62640332/155486796-57ddb063-2ff9-443b-b3e5-416ea7701ee7.png)

![image](https://user-images.githubusercontent.com/62640332/155486848-b4a0125f-74dd-41af-8798-f846f8bfdc53.png)

특정 시간에 트래픽 증가

ㅁ Technical Challenges

1. Lack of Availability& Flexibility : 서비스 확장에 따른 가용성과 유연성 확보
2. Overload On persistency Layer :  DB부하 줄여 서비스 안정적으로 제공
3. Tight Coupling : 서비스 강한 결합을 제거하여, 장애에 전파를 방지

1번 해결하기 위해서 Microservice 기반으로 전환

![image](https://user-images.githubusercontent.com/62640332/155487249-04b89188-004f-4576-94c8-0350372f80ec.png)

기존 주요 어플리케이션이 하나의 DB를 사용하는 Monolithic Architecture 였으며,

메인 DB는 온프레미스 환경에서 운영되고 있었다.

![image](https://user-images.githubusercontent.com/62640332/155487373-528d9945-8f9a-4f5c-8888-066e02d7d9df.png)

![image](https://user-images.githubusercontent.com/62640332/155487505-ec1a754d-7be6-4c3a-86b1-26ac258269f2.png)

각 마이크로 서비스는 도메인에 최적화된 가용성과 유연성을 확보할수 있게 됨.

<br>
<br>
<br>

2번 해결하기 위해서는 Cached Architecture 사용

![image](https://user-images.githubusercontent.com/62640332/155487678-b828eea9-c666-4b49-9cfe-6e0fb8a1a611.png)

데이터 영역의 높은 복잡도는 마이크로 서비스 아키텍처 전환되면서 개선 되었으나

마이크로 서비스 아키텍처로 인한 내부 트래픽이 증가하면서 각 도메인 별로 DB 부하가 높은 수준으로 증가함.

![image](https://user-images.githubusercontent.com/62640332/155487900-3cf02d0d-bd76-4585-8147-b0f9fb0d3de1.png)

자주 변경되지 않거나, 실시간 반영이 필요없는 데이터의 경우 캐시 layer 추가하여 db부하 최소화

트래픽이 증가하여도, 안정적인 서비스 가능하게 되었습니다.

<br>
<br>
<br>

도메인간의 강한 결합으로 인해서 작은 실수나 서비스의 큰 장애로 되었습니다.

이것을 최소화 하기위해  Event-Driven Architecture & CQRS 도입

![image](https://user-images.githubusercontent.com/62640332/155488328-4b2999ea-a342-4f96-82bd-b7313555ca63.png)

단계별로 마이크소서비스화 하면서 API 호출 구조가 매우 복잡해짐

이러한 구조에서는 하나의 마이크로 서비스에서 다른 서비스로 쉽게 이슈가 전파되었고,

작은 이슈에도 전체로 확대 되었습니다.

![image](https://user-images.githubusercontent.com/62640332/155488508-0618d36a-b047-43ad-a461-255f43e40609.png)

처음에는 AWS Management인  SNS, Amazo MQ, SQS 등을 추가하였고, 자체 제작한 Kafka도 추가

![image](https://user-images.githubusercontent.com/62640332/155488594-840d10af-d56e-46c8-adba-d2629a221e83.png)

또한 CQRS 도입하여, 조회 와 명령 분리하여 상호간의 영향을 최소화 하였고, 지속적인 최적화를 통해 성능 및 확정성 높이고, 보안적으로도 안정적으로 만듬.

도메인 목적에 맞게 개발을 가능하게 된다.

<br>
<br>
<br>

개발 조직은 DevOps 전략을 통해 각 도메인 특성에 맞게 빠르게 성장했지만, 표준화 되지않은 기술정책, 중복된 

시스템, 개발조직간의 Silo effect로 인해 문제점 보이기 시작 ==> SRE 의 등장

![image](https://user-images.githubusercontent.com/62640332/155489345-98d01abd-cce9-4073-8ff7-95f08b427aa5.png)

SRE는 표준화된 기술정책을 수립하고, 중복으로 개발되고있던 도구들을 플랫폼하기 시작

플랫폼을 개발하면서 표준화된 기술정책들이 자동으로 적용될수 있도록 하였습니다.

배포 모니터링플랫폼을 시작으로, 메세지 플랫폼, 인증권환 플랫폼 등을 제공 시작하였다.

개발조직은 이 플랫폼을 사용하여, 표준화된 보안, 인프라, 기술정책을 적용할수 있게 되어 기존보다

비지니스 개발에 더 집중이 가능하게 되었다.


![image](https://user-images.githubusercontent.com/62640332/155489405-9e78fde1-6e39-4057-a1ab-6abae86d29ac.png)

처음에는 AWS 서비스 최대한 활용하여 표준화 정책이 쉽게 반영될수 있게 하였고,

다양한 배포 전략을하여 보다 안정적으로 서비스가 릴리즈 될수 있도록 하였다.


![image](https://user-images.githubusercontent.com/62640332/155489584-a6a1ac5d-077e-4fb3-a1e8-46028acd0071.png)

서비스의 개발 운영을 빠르고 안정적을 하기위해 컨테이너 환경으로 전환중이다.

![image](https://user-images.githubusercontent.com/62640332/155489751-25b09c1b-afb9-4910-9778-2b9ec04d3c64.png)

시스템 상태에 대한 가시성 확보하기 위해서 매트릭, 로긴 등의  모니터링 플랫폼을 자체적으로 구축하였고,

APM을 도입하여 시스템 문제 발생시 빠르게 파악 가능하게 되었다.

시스템적인 문제는 다양한 플랫폼을 통해 극복할수 있었지만, Sile Effect는 개발 문화를 개선 해야 했다.


![image](https://user-images.githubusercontent.com/62640332/155490412-baf54957-4ffe-4b27-a946-c97eae7b7017.png)


플랫폼을 개선하는것에서 그치지 않고, 도메인 개발팀과 함께 신뢰성을 확보하기 위해 다양한 활동 하고 있다.

아키텍처 리뷰, 변경관리, 장애관리를 통해 지속적으로 경험을 공유할수있는 환경과 문화를 만들어 가고 있다.

특히 SRE는 개발팀과함께 장애의 책임을 공유합니다. 이를통해 함께 문제를 해결하고, 재발하지 않게 노력중

![image](https://user-images.githubusercontent.com/62640332/155490594-9d570e29-b95e-4602-87be-0ffc4824e415.png)

---


![image](https://user-images.githubusercontent.com/62640332/155490836-82ac1345-8d0c-45a8-8c40-fc2bcf5ef6c9.png)


![image](https://user-images.githubusercontent.com/62640332/155490889-8e5445d5-def7-49d9-aaf6-82cdffe2cfd8.png)

그러나 전자 금융업 규제로 인해서 주문 프로세스 끝에있는 결제/정산과 같은  핀테크 서비스들은 

온프레미스 환경에서 개발/운용 되고 있었다.

그러다보니 다양한 아키텍처 적응 힘듬. 게다가 전사플랫폼들이 AWS 클라우드 기반으로 만들어 지면서

기존 온프로미스 환경을 지원하는데 어려웠다.

하지만 2018년에 전자금융감독규정 개정안이 나와서 처리가 가능하게 되었다.

ㅁ Our Challenges

1. Governace : 레퍼런스가 없는 클라우드 이용 지정보고 필요 
2. Technical : 기존 온프로미스 환경보다 높은 수준의 클라우드 보안 아키텍처가 필요

첫번째의 경우 많은 문서필요, 레퍼런스 부재 발생

두번쨰의 경우 접근 및 권환관리, 안정적인 인프라 운영, 클라우드 보안강화

- 접근 및 권환 관리 => AWS 어카운트 분리 및 IAM 이용

온프레미스는 서비스별로 물리적 네트워크를 분리하고, 인가된 사용자만 IDC 출입할수 있도록 관리

클라우드에서는 핀테크 서비스 별로 AWS 어카운트를 분리하였고, IAM을 통해서 권환 관리하고있다.

![image](https://user-images.githubusercontent.com/62640332/155492039-4ea43588-10d3-4f12-a8ac-4f2070a415ea.png)

keycloak 과 AD를 이용하여 권한관리를 하고 있으며, 접근관리 강화위해서 핀테크 서비스별로 AWS 어카운트 분리

사용자 권환에 대한 관리를 필요 최소한의 원칙에 따라 부여하기 위해 IAM으로 정의

팸키??생성 및 사용을 시스템 적으로 차단하여 보안사고 방지



- 안정적인 인프라 운영 => Infrastructure As Code & Private Netowrk Hub

안정적인 클라우드 인프라 위하여, 코드로 인프라 관리하고, 개인 네트워크 허브 구축하였다.

누가 작업하더라도 일관성 유지 및 변경에 대한 추적 필요

![image](https://user-images.githubusercontent.com/62640332/155492472-1c60d9fe-5ca4-46be-a4fd-7b7437835361.png)

이러한 문제 해결하기위해 테라폼 등의 기능 도입 ==> 많은 인프라 변경 빠르게 가능, 형상관리 통해서 변화 빠르게 파악 가능


![image](https://user-images.githubusercontent.com/62640332/155492596-7159b97f-8e86-4107-b387-40a9c17cdf8f.png)


기존에는 네트워크 연동 추가할떄마다 매쉬 구조로 복잡도가 증가하는 구조였다.

이러한 구조는 복잡도가 높아지면서 작업에 필요한 시간이 증가하고, 실수의 확률이 높았다.

이러한 문제 해결하기 위해서 네트워크 허브 구축하였고, 복잡도 낮아지고, 관리비용도 줄었다.

또한 쉽게 신규 네트워크 연동이 가능하게 되었다.

- 클라우드 보안 강화 => 클라우드 기반 보안 솔루션 적용 & 클라우드 보안 로깅 시스템 & 침해 시도 로그 활용 

온프레미스 환경보다 더 높은 보안수준을 클라우드 환경에서 필요

![image](https://user-images.githubusercontent.com/62640332/155531938-2a274eac-3d73-4fa9-8234-31dee8c608d0.png)

온프레미스와 같은 수준의 앱, 웹, DDos 보안 솔루션 필요.  클라우드 환경에서는 In-line 구조로 보안 인프라 구성시 복잡도가 높아지고, 장애 발생시 트러블 슈팅이 매우 어려움

그래서 AWS에서 제공하는 managed 서비스 활용하였고, 부족한 부분은 클라우드기반 보안 플랫폼 및 

클라우드 워크로드보호 플랫폼을 도입

클라우드에 최적화된 보안솔루션을 적용해서 클라우드 보안 아키텍처를 구성


![image](https://user-images.githubusercontent.com/62640332/155532382-c0f3f80e-6657-4273-95a7-dd1170787472.png)


멀티 어카운트 환경에서 보안로그의 보관, 복원, 접근제어 , 로그접근에 대한 감사추적 등이 필요

==> 중앙 로깅 시스템 구축

SIEM과 같은 로그분석 시스템을 통해 실시간으로 로그를 분석하고 있습니다.

![image](https://user-images.githubusercontent.com/62640332/155533462-096c964c-5358-4cad-86bb-3126f9870768.png)

위에서 설명한 클라우드 보안로깅 시스템을 통해서 침해 수집된 인바운드 아웃바운드 트래픽을 실시간으로 분석하여 빠르게 외부공격을 탐지하고 있다.

