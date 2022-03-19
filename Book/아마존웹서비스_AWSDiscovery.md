- [ㅁ 클라우드 용어](#ㅁ-클라우드-용어)
- [ㅁ EC2(Elastic Compute Cloud)](#ㅁ-ec2elastic-compute-cloud)
- [ㅁ EBS(Elastic Block Storage)](#ㅁ-ebselastic-block-storage)
- [ㅁ 보안 그룹(Security Group)](#ㅁ-보안-그룹security-group)
- [ㅁ 스토리지(Stroage)](#ㅁ-스토리지stroage)
- [ㅁ S3(Simple Storage Services)](#ㅁ-s3simple-storage-services)
- [ㅁ Glacier](#ㅁ-glacier)
- [ㅁ 네트워크 및 VPC](#ㅁ-네트워크-및-vpc)
- [ㅁ DataBase](#ㅁ-database)
- [ㅁ DNS(Domain Name System)](#ㅁ-dnsdomain-name-system)
- [ㅁ 로드밸런싱(Load Balancing)](#ㅁ-로드밸런싱load-balancing)
- [가용성 및 확장 빠른 인프라 구성](#가용성-및-확장-빠른-인프라-구성)


---

### ㅁ 클라우드 용어

ㅁ 영역

- 리전(Region) : 물리적으로 위치가 다른 나라에 서버 설치
- 가용 영역(Availability Zone) : 데이터 센터(IDC; Internet Data Center)의미하며, 하나의 리전에 다수의 가용 영역(AZ) 보유
- 엣지 로케이션(Edge Location) : CDN 서ㅣ스인 CloudFront를 위한 캐시 서버(Cache Server)emfdml ahdma


### ㅁ EC2(Elastic Compute Cloud)

![image](https://user-images.githubusercontent.com/62640332/158479115-876844ee-11cd-467d-977f-9b4b291e4599.png)

- EC2 인스턴스 유형: 범용(M 시리즈), 컴퓨팅 최적화(C 시리즈), 스토리지 최적화(I 시리즈, D 시리즈), GPU 최적화(G 시리즈), 메모리 최적화(R 시리즈)

![image](https://user-images.githubusercontent.com/62640332/158478580-211a82c9-ca5d-4296-b4f8-91e5df29170d.png)


- Amazon EC2 인스턴스 구매 옵션

![image](https://user-images.githubusercontent.com/62640332/158478604-3b9f183b-4d4d-442a-b24d-46403de2861e.png)

- 빈번하게 서버 생성하고 삭제 등을 자주 사용하는 개발환경이라면 실제 사용한 시간당 사용량 과금하는 On-Demand Instace
- 장기적으로 변경없이 1~3년간 사용하는 경우 Reserved Instance
- 단기적으로 동영상 인코딩과 같이 병렬 컴퓨팅 파워를 사용하는 서비스 Spot Instance
- 고객의 전용 하드웨어 사용을 통해 보다 보안성 높고 안정적인 클라우드 서비스 사용이 목적 Dedicated Instance

<br>
<br>

### ㅁ EBS(Elastic Block Storage)

: EC2에 연결되는 Block Level의 스토리지 서비스, 서버용 하드디스크로 생각하면 이해 쉬움.

![image](https://user-images.githubusercontent.com/62640332/158479165-83ecd3ac-6ebb-43ab-899c-b7e939d4e66e.png)

- EBS 볼륨 유형

![image](https://user-images.githubusercontent.com/62640332/158479187-d199b591-e4aa-4c9c-9ec6-0a49c8947c24.png)

EBS의 스냅샷 기능이 있어, EBS 볼륨의 데이터를 스냅샷(Snapshot)으로 만들어 S3에 백업 및 보관 가능

- EBS Snapshot 특징

1. 스내뱟 진행 과정 죽에도 EBS 나 EC2 서비스 중단 없이 기존 서비스를 즉시 사용 가능
2. EBS 볼륨의 크기 조정에 사용될수 있음, 스냅샷으로 백업 후 신규로 장착할 EBS 크기 늘려 볼륨의 사이즈 늘리기 가능
3. 스냅샷의 공유 기능을 화용하여, 권한이 있는 다른 사용자에게 공유 가능
4. 다른 리전(Region)으로 복사 가능하여, 지리적 확장이나, 데이터 센터 마이그레이션(Migratrion) 및 재해복구를 쉽게 수행

- EBS 성능과 보안성 높이기
  
1. 프로비저닝된 IOPS(Provisioned IOPS) : EBS 생성시 유형에서 성능높이기 위해 Disk의 IOPS의 성능을 지정할수 있는 기능으로, EBS_Optimized 인스턴스에서 사용 가능하며,   
 보다 높은 성능의 I/O 제공하며, 고성능의 서비스 제공에 적합한 EBS 유형

2. EBS 최적화된 인스턴스(EBS-Optimized Instance) : EBS의 Disk 서비스를 위한 전용 네트워크의 대역폭을 사용하도록 구성하여, Disk 성능을 최적화하는 기능으로,   
 EC2의 인스턴스 타입중 C, M, R 시리즈에 추가 비용없이 사용 가능하며, Provisioned IOPS를 함께 사용하여 I/O의 최대 성능 끌어내기 가능

3. EBS 암호화 : AES-256으로 암호화하여 EBS 내부 데이터 보호할수 있는 기능. 암호화 키는 AWS의 KMS에서 직접 생성하거나 기본키 사용 가능,     
 이렇게 암호화된 EBS 스냅샷은 공유 및 타AWS계정에 공유되어도 사용 불가능


### ㅁ 보안 그룹(Security Group)
: 보안그룹은 인스턴스에 대한 Inbound, Outbound의 네트워크 트래픽을 제어하는 가상의 방화벽 역할 수행

EC2 인스턴스 시작시, 각 인스턴스당 최대 5개의 보안 그룹 할당 가능

이렇게 구성된 보안 그룹은 온프라미스(On-Premise)에서 사용되는 방화벽 정책과 유사한 기능이지만, 보안 그룹은 네트워크 트래픽에 대한 허용(Allow)만 가능하다. 차단(Deny)는 불가능

차단 기능을 적용하기 위해서는 VPC의 기능 중 하나인 네트워크 ACL(Network ACL)을 통해 서브넷(Subnet)수준에서 네트워크 흐름 제어 가능

- 보안 그룹 특징
  
1. 생성 가능한 보안 그룹의 숫자와 규칙에 제한 있다. 하나의 VPC당 생성 가능한 그룹의 개수는 500개, 그룹당 추가할수 있는 규칙(Rule)의 개수 50개, 네트워크 인터페이스당 5개의 보안 그룹 적용가능   
다만 필요한 경우 AWS Support를 통해 한도 증가 요청 할수 있다.

2. 네트워크 트래픽을 위한 허용(Allow)정책은 이씅나 차단(Deny)가 없다. 위 내용과 동일

3. 인바운드 트래픽과 아웃바운드 트래픽을 별도로 제어 가능

4. 초기 보안 그룹설정에는 인바운드 보안 규칙이 없다. 그래서 처음 EC2생성후 다른 EC2와 통신원하면 EC2와의 통신을 위한 인바운드 규칙 추가하여야 하낟.

<br>
<br>

### ㅁ 스토리지(Stroage)

1. DAS(Direct Attatched Stroage) : 서버에 직접 연결하는 방식
2. NSA(Network Attached Storage), SAN(Storage Area Network) : 스토리지를 빠른 속도의 네트워크로 연결하는 방식    
 
- NSA 는 LAN(Local Area Network)을 연결하여 사용하기 때문에 비용 저렴
- SAN 는 확장 용이, 대규모 엔터프라이즈 환경에 적합, 고속의 전용 네트워크를 구성하여 빠른 속도의 스토리지 서비스 제공

NAS 와 SAN 의 차이점은  SAN은 블록 수준에 데이터 저장, NSA는 파일 단위로 데이터 접속    
OS 입장에서 보면 SAN은 일반적으로 디스크로 나타나며 별도로 구성된 스토리지용 네트워크 존재, 반면 NAS는 OS에 파일 서버로 표시.


### ㅁ S3(Simple Storage Services)

: 확장성이 뛰어나며, 무한대로 저장가능하고, 사용한 만큼만 지불하는 인터넷 기반 스토리지 서비스.   
버킷(Bucket)이라는 리전(Region)내에서 유일한 영역을 생성하고 데이터를 키-값 형식의 객체(Object)로 저장합니다.   
비용이 매우 저렴, 간단한 웹 서비스를 위한 웹사이트 만들기 가능.   

s3는 스토리지 기술을 근간으로 하며, 파일 단위로 접근만 지원하기 떄문에 EBS(Elastic Block Storage) 서비스 대체 불가능.

![image](https://user-images.githubusercontent.com/62640332/158488693-4c446cae-c974-4b67-a833-130af23dde62.png)

![image](https://user-images.githubusercontent.com/62640332/158488733-283f0761-bf02-4c5c-a134-9b7f124794d0.png)


- S3 스토리지 클래스
  
![image](https://user-images.githubusercontent.com/62640332/158488757-84da7936-f658-4674-9af1-29b37a0eb7a0.png)

1. S3 표준(S3 Standard) : 자주 액세스하는 데이터를 위한 스토리지 클래스로 내구성, 강ㅇ성, 성능이 뛰어난 객체 스토리지 서비스 제공. 비용은 EBS 대비 20% 저렴, 전송 데이터위한 SSL 및 저장 데이터 암호화 지원
2. S3 표준-IA(S3 Standard Infrequent Aceess) : 액세스 빈도는 낮지만 필요시 빠르게 액세스 해야하는 데이터를 위한 스토리지 클래스. 가격은 S3대비 58%저렴, 최근 백업 서비스에 많이 사용되는 스토리지 클래스
3. S3 One Zone-IA(S3 One Zone Infrequent Access) : 최소 3개의 가용 영역(AZ)에 데이터를 저장하는 다른 S3 스토리지 클래스와 달리, 단일 AZ에 데이터 저장하여 S3-IA 대비 20% 저렴
4. Glacier : 데이터 보관을 위한 안전하고 비용이 매우 저렴한 서비스. S3와 같은 내구성과 성능 및 가용성 보유하며, S3 표준 대비 최고 77%까지 저렴. 데이터 아카이빙 및 장기간 데이터 보관 및 오래된 로그 데이터에 대한 저장용도로 적당한 서비스. 또한 S3의 수명주기 기능을 통한 객체 자동 마이그레이션을 제공합니다.

<br>
<br>

### ㅁ Glacier

![image](https://user-images.githubusercontent.com/62640332/158597286-57d6289d-fd50-43bd-bdfa-73a128149306.png)

S3의 개별 스토리지 영역인 Bucket 과 유사한 Vault라는 개별 스토리지 영역을 생성하여 데이터 보관.

Console을 통한 업로드 지원, 별도의 API 이용하여 데이터에 대한 저장기능 제공

일반적으로 S3에 저장되는 데이터는 라이프사이클 옵션을 활용하여 일정 기간 이상 지난 데이터에 대해 보다 저렴한 Glacier로 이동하여 저장하는 옵션을 사용 가능

![image](https://user-images.githubusercontent.com/62640332/158597888-9f619bc0-3592-4ff8-9a50-e422c6ae7fc5.png)

- 데이터 접근 방법

1. API/SDK를 이용한 Direct 연결
2. S3 라이프 사이클과의 통합
3. 3Party Tool과 AWS Storage Gateway연동

![image](https://user-images.githubusercontent.com/62640332/158598148-27d24536-b3f1-4b86-af2a-6c7fe19eed0f.png)


<br>
<br>

### ㅁ 네트워크 및 VPC

- AMI(Amazon Machine Image) : EC2 인서튼서 생성에 필요한 몯느 소프트웨어 정보를 담고 있는 템플릿 이미지
- Amazon Marketplace : AWS에서 실행되는 소프트웨어 판매 또는 구매할 수 있는 온라인 스토어
- VPN(Virtual Private Network) : 보안성 높은 사설 네트워크(Private Network)를 만들거나, 원격지간 서로 연결하고 암호화 기술 적용하여 보다 보안성 높은 통신서비스 제공,    기존 IDC에서 서비스하던 모든 시스템을 클라우드로 이전하는 것은 매우 어려운일, 그래서 AWS는 VPC(Virtual private Cloude)와 VPC Gateway를 통해 On-premise의 VPN장비와 AWS간의 VPn 연결 가능하게 하여, 보안성 높은 하이브리드 클라우드(Hybrid Cloud)환경 구현
- VPC(Virtual Private Cloude) : AWS클라우드에서 논리적으로 격리된 네트워크 공간을 할당하여 가상 네트워크에서 AWS리소스를 이요할수 있는 서비스 제공

![image](https://user-images.githubusercontent.com/62640332/158611824-6549e12e-0740-48c2-ac19-13e2a31a635d.png)

![image](https://user-images.githubusercontent.com/62640332/158611954-2a8f0254-2858-4c6a-b757-b25dbece81a5.png)

ㅁ VPC 구성요소

    - 프라이빗 IP주소 : 인터넷을 통해 연결할 수 없는, VPC 내부에서만 사용할 수 있는 IP주소, VPC에서 시작된 인스턴스 서브넷의 범위에서 자동으로 할당되며, 동일 네트워크에서 인스턴스 간 통신에 사용 가능. 기본 프라이빗 주소와 별도로 보조 프라이빗 IP주소라는 추가 프라이빗 주소를 할당가능
    - 퍼블릿 IP주소 : 인터넷을 통해 연결가능한 IP주소로, 인스턴스와 인터넷간의 통신을 위해 사용 가능. EC2 생성시 옵션으로 퍼블릭 IP주소 사용여부 선택 가능하며, 인스턴스에서 퍼블릭 IP주소를 수도응로 연결하거나 해제 불가능. 또한 인스턴스가 재부팅되면 새로운 퍼블릭 IP주소 할당
    - 탄성 IP주소 : 동적 컴퓨팅을 위해 고안된 고정 퍼블릭 IP주소. VPC의 모든 인스턴스와 네트워크 인터페이스에 탄성 IP할당 가능, 다른 인스턴스에 주소를 신속하게 다시 매칭하여 인스턴스 장애 조치 수행 가능. 탄력적 IP 주소의 효율적인 활용위해 탄력적 IP 주소가 실행중인 인스턴스와 연결되어 있지 않거나, 중지된 인스턴스 또는 분리된 네트워크 인터페이스와 연결되어 있는 경우 시간당 요금 부과. 사용가능한 탄력적 IP 주소는 5개로 제한, 이를 절약하기 위해 NAT 디바이스 사용 가능

<br>

- VPC와 서브넷(Subnet)

: VPC는 사용자의 AWS 계정을 위한 전용의 가상 네트워크. VPC는 AWS 클라우드에서 다른 가상 네트워크와 논리적으로 분리되어 이씅며, EC2 인스턴스와 같은 AWS 리소스를 VPC에서 실행할 수 있습니다.

VPC 내부의 네트워크에서도 서비스 목적에 따라 IP Block으로 나누어 구분 가능. 우리는 이렇게 불리된 IP Block의 모음을 `서브넷(Subnet)`이라고 합니다.

VPC는 리전(Region)의 모든 가용 영역(AZ)에 적용되며, 각 가용 영역에 하나 이상의 서브넷 추가 가능. 하지만 서브넷은 단일 가용 영역에서만 생성가능하며, 여러 가용 영역으로 확장 불가능

<br>

- VPC와 서브넷의 사이즈

: VPC생성시 사용하게될 IP 주소의 범위를 지정하게 되는데 이 범위를 `CIDR(Classless Inter-Domain Routing)` 블록 형태로 지정해야 함.

<br>

- 퍼블릭 서브넷(Public Subnet)과 프라이빗 서브넷(Private Subnet)

: 서브넷 네트워크 트래픽이 인터넷 게이트웨이(Internet Gateway, IGW)로 라우팅 되는 서브넷을 `퍼블릭 서브넷(Public Subnet)`이라 하고, 인터넷 게이트웨이로 라우팅 되지 않는 서브넷을 프라이빗 서브넷(Private Subnet)이라 합니다.

EC2 인스턴스가 IP를 통해 인터넷과 통신할려면, 퍼블릭 IP주소나 탄력적 IP(Elastic IP) 주소가 있어야 합니다. 일반적으로 인터넷망을 통해 서비스를 수행하는 웹서버(Web Server)는 퍼블릭 서브넷에 생성하며, 인터넷에 직접적으로 연결할 필요가 없고, 보다 높은 보안성을 필요로하는 DB 서버는 프라이빗 서브넷에 생성합니다.

<br>

- 라우팅 테이블(Routing Table)

: 각 서브넷은 서브넷 외부로 나가는 아웃바운드 트래픽에 대해 허용된 경로를 지정하는 라우팅 테이블(Routing Table)이 연결되어 있어야 한다. 생성된 서브넷은 자도응로 VPC의 기본 라우팅 테이블과 연결되며, 테이블의 내용을 변경할 수 있습니다.

서브넷 간의 통신이나 VPC간의 원활한 통신을 위해 라우팅 테이블 이용

<br>

ㅁ VPC 주요 서비스

- 보안 그룹(Security Group)과 네트워크 액세스 제어 목록(Network ACL)

: VPC는 테으워크 통신과 트래픽에 대해 IP 와 Port 기준으로 통신 허용, 차단 기능 제공. 이러한 서비스를 보안 그룹(Security Group)과 네트워크 ACL(Network ACL)이라고 합니다.

![image](https://user-images.githubusercontent.com/62640332/158627868-57788a8e-67c1-4ce2-99be-3879f6f32cda.png)

차이가 있으므로 선택적으로 적용하여 사용하는것 권장

- VPC 피어링 연결(VPC Peering Connection)

: 피어링 연결은 비공개적으로 두 VPC 간에 트래픽을 라우팅할 수 있게 하기 위한 서로 다른  VPC 간의 네트워크 연결을 제공합니다. VPC Peering을 통해 동일한 네트워크에 속한 것과 같이 서로 다른 VPC의 인스턴스 간에 통신이 가능

 
![image](https://user-images.githubusercontent.com/62640332/158628291-3cb67276-d2fc-40e6-8948-1841ce8d4cf1.png)


- NAT(Network Address Translation) 게이트웨이

: 외부 네트워크에 알려진 것과 다른 IP 주소를 사용하는 내부 네트워크에서, 내부 IP 주소를 외부 IP 주소로 변환하는 작업을 수행하는 서비스

NAT 게이트웨이는 프라이빗 서브넷(Private Subnet) 내에 있는 인스턴스를 인터넷 또는 다른 AWS 서비스에 연결하고, 외부망 또는 인터넷에서 해당 인스턴스에 연결하지 못하도록 구성하는데 사용

외부에 공개될 필요가 없거나, 보안상 중요한 서비스이지만 윈도우 패치나 보안 업데이트, 소프트웨어 업데이트를 인터넷을 통해 받아야 하는 경우 NAT 게이트웨이나 NAT 인스턴스(NAT Instance)를 사용하게 됩니다.

NAT 게이트웨이 구성하기 위한 3가지 조건

1. NAT 게이트를 생성하기 위해 퍼블릭 서브넷(Public Subnet)을 지정
2. NAT 게이트웨이와 연결할 탄력적 IP(Elastic IP)주소 필요
3. NAT 게이트웨이를 만든 후 인터넷 트래픽이 ANT 게이트웨이로 통신이 가능하도록 프라이빗 서브넷(Pirvate Subnet)과 연결된 라우팅 테이블(Routing table) 업데이트

 
![image](https://user-images.githubusercontent.com/62640332/158629118-bc2e3571-efca-46ae-b38a-03555bdda48e.png)

이와 같이 NAT 게이트웨이를 구성하면 프라이빗 서브넷의 인스턴스가 인터넷과 통신할 수 있습니다.

<br>

- VPC Endpoint

: S3는 인터넷망에 연결된 서비스로 인터넷 기반의 IP 주소와 연결 정보를 가지고 있다. 이러한 공용 리소스에 대해 퍼블릭 서브넷(Public Subent)에 위치한 인스턴스는 인터넷을 통해 문제 없이 연결 가능합니다.

하지만 프라이빗 서브넷(Private Subnet)에 위치한 인스턴스는 인터넷과 연결되어 있는 S3와 같은 공용 리소스르 연결 불가능.

 
![image](https://user-images.githubusercontent.com/62640332/158629753-f843db3f-d1fd-41ef-9c6b-07d346c81264.png)

이러한 경우 S3 연결 위해서는 NAT 게이트웨이 나 NAT 인스턴스가 필요. 하지만 VPC Endpoint를 이용하면 빠르고 손쉽게 S3, DynamoDB에 연결할 수 있습니다.

<br>

- VPN 연결

: 기본적으로 VPC에서 서비스되는 인스턴스는 On-Premise에 있는 서버나 IDC 내의 시스템과 통신 불가능. 물론 인터넷을 통해 강제로 통신하도록 구성은 가능하지만, 보안을 필요로 하는 중요한 데이터를 송수신하기에는 보안적으로 매우 취약

![image](https://user-images.githubusercontent.com/62640332/158630222-e12d11e4-0e52-4151-827f-f62220527a52.png)

이렇게 VPC 내 인스턴스와 IDC 내 시스템간의 데이터 통신을 위해 VPC에 가상의 프라이빗(Private) 게이트웨이를 연결하고 사용자 지정 라우팅 테이블을 생성하며, 보안 그룹의 규칙을 업데이트하고, AWS 관리형 VPN 연결을 생성하여 VPC에서 원격의 네트워크 접속 가능하도록 하이브리드 클라우드(Hybrid Cloud) 환경을 구성할 수 있습니다. VPN 연결은 VPC와 자체 네트워크 사이의 연결을 의미

\# VPC 내부에서는 트래픽에대해 비용이 발생하지 않지만, VPC 외부로 데이터 송신하는 경우 사용량에 비례하여 OutBound 트래픽에 대한 비용이 발생



### ㅁ DataBase

- RDS(Relational Database Services)

: 클라우드에서 관계형 데이터베이스를 더욱 간현하게 설정, 운영 및 확장 할수 있는 서비스

Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server 등 익숙한 데이터베이스 엔진 중에서 원하는 DBMS 선택 가능.

![image](https://user-images.githubusercontent.com/62640332/158838977-15f78e72-b197-4c4b-b024-d10109f91612.png)


Amazon 클라우드 데이터베이스 서비스 선택사항

1. 직접 EC2에 DB설치하여 이용하는것
2. AWS에서 직접 제공해주는 DB 서비스 이용

![image](https://user-images.githubusercontent.com/62640332/158839186-c54cdff3-e6e2-4405-aeab-58a90e58cd70.png)


- RDS 주요특징

1. 유연한 인스턴스 및 스토리지 확장
2. 손쉽게 사용 가능한 백업 및 복원 기능 : DB는 최대 35일 데이터 보존 가능, 백업된 스냅샷(Snashot)을 통해 DB 생성도 가능
3. 멀티 AZ(Availability Zone)을 통한 고가용성 확보
4. RDS 암호화(Encryption) 옵션을 통한 보안성 강화
5. Database Migration 서비스


### ㅁ DNS(Domain Name System)

: 도메인 이름을 IP 주소로 변환하여, 웹 사이트로 트래픽을 라우팅하는 역할


![image](https://user-images.githubusercontent.com/62640332/158842217-48bd883c-a0d2-46f5-b4f9-1b83bd4c4349.png)

![image](https://user-images.githubusercontent.com/62640332/158842369-5a03d5f1-abea-41d4-98b3-282400c89365.png)

도메인 체계에서 최상위는 `Root`, 그아래 단계를 `1단계 도메인`이라고 하며, 이를 최상위 도메인 또는 TLD(Top Level Domain)이라 한다.

최상위 도메인은 국가명을 나타내는 국가 최상위 도메인과 일반적으로 사용되는 일반 최상위 도메인으로 구분.

도메인 구입한경우 1단계 도메인 중 하나를 선택하고, 원하는 도메인명 지정하여 등록.


- DNS의 동작원리 
   
![image](https://user-images.githubusercontent.com/62640332/158842740-a49a4faa-4da1-40a6-986a-1c626203e145.png)

![image](https://user-images.githubusercontent.com/62640332/158842969-c00e9a7c-f3df-4e01-b5d6-6707abace748.png)

ㅁ Route 53

: 가용성과 확작성 우수한 클라우드 기반의 DNS 웹서비스. 

![image](https://user-images.githubusercontent.com/62640332/158843183-b4d03609-db9d-4226-9e85-890872dd56f0.png)

![image](https://user-images.githubusercontent.com/62640332/158843223-cb21ae99-bd1f-4ceb-aa9a-1e3fcae639b5.png)


<br>

- Route 53의 주요 특징 및 기능

![image](https://user-images.githubusercontent.com/62640332/158843307-fa8cf734-da5f-4fb2-978a-4e7faa2c7be0.png)

: route 53의 HEalth check 기능을 사용하면 상태 확인 에이전트가 route53에 연결된 응요프로그램의 각 끝점을 모니터링하여 서비스의 사용 가능 여부 확인하고, '정상' or '비정상' 반환

이를 사용해서 외부 사용자가 직접 접속한 것과 유사한 상황을 시뮬레이션(Simulation)할 수 있습니다.

리소스의 연결 상태가 좋지 않을 떄 알림 메일을 수신하도록 각 상태 검사에 대해 CloudWatch알림을 구성할 수 있습니다.

또한 장애 최(DNS Failover)가 구성되어 있고 에이전트가 정상이 아닌것으로 판단되면 Route 53는 외부 사용자를 정삭적으로 연결 가능한 사전 정의된 대체 서버나 지정된 앤드포인트(End-Point)로 연결을 전환시킬수 있습니다.


<br>

- 고가용성 DNS(High Availability DNS) 서비스 및 DNS Failover

: route53는 상태 검사(health Check(와 연결된 장애 조치(Failover) 레코드 구성 가능.

 
![image](https://user-images.githubusercontent.com/62640332/158844034-adb9c90f-49b8-49e6-a888-abc83fc5b4d0.png)

상태 검사에서 연결 상태가 정상 상태가 반환되면, 응용프로그램은 계속 정상적으로 작동합니다.

상태 검사에서 연결 사앹가 비정상 상태로 반환되면 Route 53에서 정상 상태가 아닌 끝점 값을 반환하지 않고 오류 복구 레코드의 값에 대해 응답하기 시작.

DNS Failover Record를 활용하면 외부 사용자를 으용프로그램의 오류나 시스템 장애 상황에서 미리 정의된 응용프로그램이나 정상적으로 도달 가능한 외부 리소스 로 연결을 전환합니다.

이렇게 으용프로그램이나 시스템의 장애 사오항에서 정상적인 앤드포인트(End-Point)로 장애 조치(Failover)를 수행하면 웹 사이트 또는 애플리케이션의 다운 타임을 최고하 가능.

 
![image](https://user-images.githubusercontent.com/62640332/158844462-cd5e2bd3-0f06-40f5-acad-df786fbc2605.png)

<br>

- 지연 시간 기반 라우팅(Latency Based Routing)

![image](https://user-images.githubusercontent.com/62640332/158844552-efe7f19c-5df2-43bc-a79c-e37767dfc9d3.png)

: route53는 동일한 기능을 수행하는 여러 데이터 센터에 EcC2 리소스가 있고, 가장 지연시간이 적은 리소스로 route53에서 DNS쿼리에 응답 처리하여 지연 시간 기반 라우팅 서비스 제공.

지연 시간 기반 라우팅은 최종 사용자에게 최저 지연 시간을 제공하는 엔드포인트로 라우팅을 제공하여, 일정 기간동안 수행된 지연 시간 측정을 기반으로 하며 주기적으로 자연시간을 측정하여 변경 사항을 반영합니다.


<br>

- 가중치 기반 라우팅(Weighted Round Robin Routing)

 
![image](https://user-images.githubusercontent.com/62640332/158844892-907165d2-e3df-4832-8150-a5bdd999d474.png)

: 여러 리소스 레코드를 단일 DNS 이름으로 연결후 같은 기능을 수행하는 여러 리소스에 대해 사용자가 지정한 가중치 비율로 트래픽 운용 가능.

가중치 기반 라우팅은 로드 밸런싱 및 새 버전의 소프트웨어 테스트를 포함하여 다양한 목적에 사용됩니다.

<br>

- 지역 기반 라우팅(Geolocation Routing)

![image](https://user-images.githubusercontent.com/62640332/158845139-af27e409-5c1e-4455-8463-8a7764480757.png)

: 요청이 시작된 지리적 위치를 기반으로 특정 엔드포인트에 대한 라우팅을 수행하는 기능.

국가별 또는 사용자의 지역적 위치에 따라 현지화된 콘텐츠를 사용자별로 제공하거나, 라이선스가 있는 시장에만 콘텐츠 배포를 한정하거나 배포 대상으로 선택할 수 있습니다.

<br>
<br>

### ㅁ 로드밸런싱(Load Balancing)

: 네트워크 기술의 일종으로 네트워크 트래픽을 하나 이상의 서버나 장비로 분산하기 위해 사용되는 기술.

로드 밸런싱을 수행하는 소프트웨어나 하드웨어를 로드 밸런서(Load Balancer)라고 합니다.


![image](https://user-images.githubusercontent.com/62640332/159125667-3dd38364-46d5-45ec-bb9a-beea01e3fb92.png)

- 일반적인 웹 트래픽 증가에 대한 처리 방식

1. Scale Up : 기존보다 높은 성능을 보유한 웹서버로 시스템을 업그레이드함으로써 문제를 해결하는 방식, 성능 높아질수록 비용 기하급수적 증가, 하나의 웹서버에서 웹 서비스를 제공하여 서버 중지 및 장애로 인해 웹 서비스 가용성 문제 발생 가능
2. Scale Out : Cluster로 구성하는 경우 Cluster 내 하나의 노드에 문제가 발생하여도 웹 서비스가 중단되지 않으므로 가용성이 높은 웹서비스를 구성할 수 있습니다. 로드밸렁싱은 Scale-Out 방식의 웹 서비스 구성에 주로 사용되며, 네트워크 트래픽을 서비스의 Port 단위로 제어하고, 트래픽을 분산처리함으로써 높은 가용성과 부하 분산을 통한 고효율 웹서비스를 제공합니다.


- 로드 밸렁싱 방식

1. Roun Robin : Real 서버로서의 Session 연결을 순차적으로 맺어주는 방식. 연결된 Session 수에 상관 없이 순차적으로 연결. Session에 대한 보장 제공 X
2. Hash : Hash 알고리즘을 이용한 로드밸런싱 방식. Client 와 Server 간에 연결된 Session을 계속 유지해 주는 방식으로 Client가 특정 Server로 연결된 이후 동일 서버로만 연결되는 구조. Session에 대한 보장 제공
3. Least Connection : Session 수를 고려하여 가장 적은 Session을 보유한 서버로 Session을 맺어주는 연결방식. Session에 대한 보장 제공 X
4. Response Time : 서버 간의 Resource와 Connection의 차이가 있는 환경에서 사용되는 방식으로 응답시간을 고려하여 빠른 응답시간을 제공하는 서버로 Session을 맺어주는 방식. Session에 대한 보장 제공X

- Elastic Load Balancing

: 단일 가용 영역 또는 여러 가용 영역에서 EC2인스턴스 및 컨테이너, IP 주소 같은 동일한 서비스를 제공하기 위해 준비된 여러 대상으로 애플리케이션 및 네트워크 트래픽을 자동으로 분산시킵니다.

Load Balacing은 서비스의 복적에 따라 세 가지의 로드 밸런서 중 하나의 서비스를 선태갛여 사용할 수 있으며, 이를 통해 애플리케이션의 내결함성 보장을 위해 필요한 고가용성, 부하분산, 자동 확대/축소, 강력한 보안 기능을 제공

![image](https://user-images.githubusercontent.com/62640332/159126044-41df3696-1e38-4a25-a20c-1486f88bd644.png)


  - ELB 요구 사항에 따라 서비스 선택

![image](https://user-images.githubusercontent.com/62640332/159126065-17d15049-368c-499e-a6b3-ac0ea25c5f0e.png)

<br>

  - 로드 밸런싱 서비스를 인터넷에 견결한 것인지에 따라 서비스 선택

![image](https://user-images.githubusercontent.com/62640332/159126083-f6f5d9f4-3ad5-4f9b-a722-b5231aae19c4.png)


<br>

- ELB의 주요특징

1. 상태 확인 서비스(Health Check) : ELB와 연결된 인스턴스의 연결 상태를 수시로 체크하여 인스턴스의 OS나 애플리케이션의 문제로 인해 연결 장애니ㅏ 서비스 가능 여부에 대한 Health Check를 지속적으로 수행. 이러한 Health check가 실해하는 경우 해당 인스턴스로 트래픽을 전달하지 않습니다. 이를 위해 HTTP와 HTTPS 상태 확인 빈도, 실패 임계치, 성공 시 응답코드를 임의 설정 가능하며, 자세한 상태 확인과 실패원인은 API를 통해 확인은 물론 AWS 콘솔에도 표시됩니다. TCP 방식으로 Health Check를 수행하는 경우 서비스 Port의 오픈 여부 및 연결 가능 여부 확인하며, HTTP나 HTTPS 방식은 특정 웹 페이지의 접속 시도에 따른 응답 코드(200)가 정상 반환 여부를 확인해서 Health Check 성공/실패 여부 판단.

![image](https://user-images.githubusercontent.com/62640332/159127613-c59d6390-5a87-40a5-9057-c729ffde81f2.png)

2. Sticky Session : ELB를 통해 트래픽을 부하 분산하는 경우 기본적으로 Round Robin 방식으로 트래픽 분산. 이 경우 한 번 연결된 Session은 다음 연결 시 그대로 연결되지 않으며, 다음 연결 시 다른 인스턴스로 연결될 수 있어서 애플리케이션의 Session을 유지할 수 없게 됩니다. 특히 세션 유지가 필요한 백오피스 웹 사이트의 경우 연결이 끊어지거나 웹사이트의 로그인 및 인증 정보를 유지할 수 없게 됩니다. Stick Session을 사용하면 처음 연결된 Clinet에 별도의 HTTP 기반의 쿠키 값을 생성하여 다음번 연결 요청에 대해 처음 접속했던 서버로 계속 연결하도록 트래픽 처리.

![image](https://user-images.githubusercontent.com/62640332/159128653-7a95b10f-f445-4ebd-afc4-9cf9b6d7498f.png)

3. 고가용성 구성 : ELB는 단일 가용 영역 또는 여러 가용영역에있는 여러 대상(EC2 인스턴스, 컨테이너 및 IP)에 걸쳐 트래픽을 자동 분산 가능. 특히 고가용성 구성을 위해 Route53와 같은 다른 서비스와의 연계를 통해 가용성 서비스 제공 가능.

![image](https://user-images.githubusercontent.com/62640332/159128901-affaf63a-1f1e-43d5-867d-9c4617692995.png)

4. SSL Termination 및 보안 기능 : 웹 사이트에 SSL 인증서를 적용하여 HTTPS와 같은 방식으로 암호화 통신을 하기 위해서는 개별 웹 서버에 별도의 공인인증서를 구매 후 적용해야한다. 이 경우 개별 인증서를 각 인스턴스에 직접 적용은 물론 인증서 만료에 따른 갱신 등 관리가 필요합니다.    또한 개별 EC2 인스턴스에서 SSL 암호화 및 복호화를 직접 처리하므로 인스턴스에 암호화및 복호화 처리를위한 추가 부하가 발생됩니다. ELB의 SSL Termination 기능을 사용하게 되면 개별 인스턴스에 SSL 인증서를 직접 설치할 필요X. ELB의에 공인인증서 또는 ACM(Amazon Certifacate Manager)에서에서 무료로 발급받을 수 있는 사설 인증서를 등록함으로써, SSL 인증서를 이용한 HTTPS 활용 트래픽 암호화 및 복호화 서비스를 제공할 수 있습니다.     ACM(Amazon Certificate Manager)을 사용하는 경우 추가적인 인증서 발급 비용은 무료이며 별도의 인증서 관리도 필요 없습니다. 그리고 인스턴스의 암호화 및 복호화에 따른 부하를 줄일수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/159129981-7bfb3b35-ee95-4600-b124-aaddd15b234d.png)

### 가용성 및 확장 빠른 인프라 구성

- 가용성(Availability) : 해당 시스템이나 서비스가 가동 및 실행 되는 시간의 비율을 말한다.    중요한 업무 시스템이나 평상시 서비스 중지 및 다운타임을 가져갈 수 없는 시스템을 설계해야하는 경우 인프라의 가용성을 극대화 할 수 있는 아키텍처로 인프라를 구성합니다. 이러한 시스템을 '고가용성 시스템'이라 한다.

- 확장성(Scalability) : 서비스나 응용프로그램이 증가하는 성능 요구에 맞게 향상될 수 있는 정도를 나타냅니다.   시스템은 사용자 증가에 따라 시스템의 자원이나 리소스를 손쉽게 추가/삭제할 수 있습니다.   이러한 확장성은 물리적 하드웨어 환경에서 스케일 업(Scale up)과 스케일 아웃(Scale out)이라는 두가지의 확장성 전략을 이용하여 구현할 수 있다.

스케일 업은 하드웨어에 대해 시스템 리소스(프로세서, 메모리, 디스크, 네트워크 어댑터 등) 추가하거나 기존 하드웨어를 더욱 강력한 것으로 교체하는 작업 포함

스케일 아웃은 서버를 여러 대 추가하여 처리 능력을 향상시키는 방법.


- Auto Scaling : 인스턴스틀 늘려 성능을 유지하고, 이용자가 줄어 평상시 상황이 유지되면 인스턴트를 자동으로 줄여 비용을 줄이는 효과를 볼수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/159132612-6dc22261-23b5-452c-96aa-41e224592d28.png)


![image](https://user-images.githubusercontent.com/62640332/159132647-417fcfa9-efd9-4c2e-9a0b-bff89e27e602.png)


