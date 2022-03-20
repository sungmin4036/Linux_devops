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
- [ㅁ 가용성 및 확장 빠른 인프라 구성](#ㅁ-가용성-및-확장-빠른-인프라-구성)
- [ㅁ CDN(Contents Delivery Network)](#ㅁ-cdncontents-delivery-network)
- [ㅁ IAM(identity & Sccess Management)](#ㅁ-iamidentity--sccess-management)
- [ㅁ 호스팅(Hosting)](#ㅁ-호스팅hosting)

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

### ㅁ 가용성 및 확장 빠른 인프라 구성

- 가용성(Availability) : 해당 시스템이나 서비스가 가동 및 실행 되는 시간의 비율을 말한다.    중요한 업무 시스템이나 평상시 서비스 중지 및 다운타임을 가져갈 수 없는 시스템을 설계해야하는 경우 인프라의 가용성을 극대화 할 수 있는 아키텍처로 인프라를 구성합니다. 이러한 시스템을 '고가용성 시스템'이라 한다.

- 확장성(Scalability) : 서비스나 응용프로그램이 증가하는 성능 요구에 맞게 향상될 수 있는 정도를 나타냅니다.   시스템은 사용자 증가에 따라 시스템의 자원이나 리소스를 손쉽게 추가/삭제할 수 있습니다.   이러한 확장성은 물리적 하드웨어 환경에서 스케일 업(Scale up)과 스케일 아웃(Scale out)이라는 두가지의 확장성 전략을 이용하여 구현할 수 있다.

스케일 업은 하드웨어에 대해 시스템 리소스(프로세서, 메모리, 디스크, 네트워크 어댑터 등) 추가하거나 기존 하드웨어를 더욱 강력한 것으로 교체하는 작업 포함

스케일 아웃은 서버를 여러 대 추가하여 처리 능력을 향상시키는 방법.


- Auto Scaling : 인스턴스틀 늘려 성능을 유지하고, 이용자가 줄어 평상시 상황이 유지되면 인스턴트를 자동으로 줄여 비용을 줄이는 효과를 볼수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/159132612-6dc22261-23b5-452c-96aa-41e224592d28.png)


![image](https://user-images.githubusercontent.com/62640332/159132647-417fcfa9-efd9-4c2e-9a0b-bff89e27e602.png)


- Auto Scaling의 구성요소
  
1. Auto Scaling 그룹 : 인스턴스의 조정 및 관리 목적으로 구성된 논리적 그룹으로 AUto Scaling을 수행하는 인스턴스의 모음.

![image](https://user-images.githubusercontent.com/62640332/159150405-a8ddd858-c35b-42f5-9f46-75b40c29526c.png)


2. 시작 구성 : 시작 구성은 Auto Scaling 그룹에서 인스턴스를 싲가하는 데 사용하는 템플릿. 시작 구성을 생성하는 경우 AMI, 인스턴스 유형, 키페어, 하나 이상의 보안 그룹, EBS 등 인스턴스에 대한 정보를 지정

시작 구성은 여러 개의 Auto Scaling 그룹에 지정될 수 있느나, Auto Scaling 그룹은 하나의 시작구성만을 지정할 수 있습니다. 또한 시작 구성은 한 번 생성한 이후에는 수정/변경할 수 없습니다. 따라서 시작 구성을 변경하여 Auto Scaling 그룹에 적용하고자 한다면, 시작 구성을 새롭게 생성하여  Auto Scaling 그룹을 업데이트 해야한다.

3. Auto Scaling 그룹 조정: 인스턴스의 수를 늘리거나 줄이는 기능. 조정 작업은 이벤트와 함께 시작 되거나, AUto Scaling 그룹의 인스턴스를 시작하거나 종료하도록 수행하는 조정 자겅ㅂ과 함께 수행됩니다.

![image](https://user-images.githubusercontent.com/62640332/159150481-3202fdfb-0f9b-4a24-a571-d4ffcae96b06.png)

  - 인스턴스 조정 옵션

    - 현재 인스턴스 수준 유지 관리 : 최소 또는 항상 지정된 수의 인스턴스를 실행 유지 관리하도록 구성가능
    - 수동 조정 : Auto Scaling 그룹에서 최소, 최대 또는 원하는 용량의 변경 사항 조정 변경 가능
    - 일정을 기반으로 조정 : 예층 가능한 일정에 따라 수요가 증가하거나 감소하는 경우 일정에 따른 확장 및 축소 작업을 시간 및 날짜 함수를 통해 자동으로 수행되도록 구성 가능
    - 온디맨드 기반 조정 : 리소르를 조정하는 가장 효과적인 방법으로 인스턴스의 CPU 사용률이 15둥 동안 90% 유지될 떄마다 인스턴스를 확장하도록 구성하는 정책을 생성할 수 있다.  이는 변화하는 조건에 따라 효과적으로 자원의 조정을 가능하게 합니다. CPU, 메모리 사용량, 네트워크의 대펵폭이 일정 수준이상인 경우 새로운 인스턴스를 시작하고, 네트워크 ㄷ역폭이 다시 내려가면 인스턴스를 종료하는 정책을 수립하여 적용 가능. 이러한 모니터링 기반의 조정은 2개의(확장/축소) 정책을 통해 작업을 수행.
  

### ㅁ CDN(Contents Delivery Network)

: CDN은 Contents Deliver Network 또는 Content Distribution Network의 약자로 콘텐츠를 효율적으로 전달하기 위해 여러 노드를 가진 네트워크에 데이터를 저장하여 제공하는 시스템.  이러한 CDN을 통해 온라인 상의 대용량 콘텐츠를 저렴한 비용으로 빠르게 전송하도록 합니다. 주요 ISP(Internet Service Provider)의 CDN 서버에 콘테츠를 분산시키고 유저의 네트워크 경로 상 가장 가까운 곳의 서버로부터 콘텐츠를 전송받도록 하여 트래픽이 특성 저버에 집주오디지 않고 각 지역 서버로 분산되도록 하는 기술.

![image](https://user-images.githubusercontent.com/62640332/159150775-eca77844-970c-4d5e-a917-3a8ae92606e1.png)


- CDN의 동작 원리 : PC나 모바일 기기의 엡 브라우저에서 URL을 이용하여 웹 사이트에 접속을 시도하게 된면, 사용자에게 웹 페이지를 제공하기 위해 필요한 콘텐츠(HTML, 이미지, CSS, javaScript 등)를 서버에 요청합니다. 대부분의 CDN 서비스는 콘텐츠에 대한 요청이 발생하게 되면 사용자(End-User)와 가장 가까운 위쳉 배치도니 CDN 서버로 사용자를 접속시키게 되며, CDN 서버는 요청된 파일의 캐싱된(사전 저장된) 콘텐츠를 사용자에게 전달하게 됩니다.

![image](https://user-images.githubusercontent.com/62640332/159150837-f352ab82-5140-4182-9e0b-658b96045370.png)

서버가 파일을 찾는데 실패하거나, 콘텐츠가 너무 오래걸리는 경우 오리진(원본) 서버에서 파일을 조회하여 사요앚에게 전달하여, 이후 동일한 콘텐츠를 요청받게 되면 캐싱된 데이터에서 콘텐츠를 전송하므로 보다 빠르게 전달 가능.

- CDN 캐싱 박싱의 종류

: 크게 Static Caching과 Dynamic Caching으로 나눌수 있습니다.

  - Static Caching은 사용자의 요청이 없어도 Origin Server에 있는 Contents를 운영자가 미리 Cache Services에 복사함으로써 사용자가 Cache 서버에 접속하여 Contents를 요청하면 Cache 서버가콘텐츠를 전달하는 방식.

![image](https://user-images.githubusercontent.com/62640332/159150977-50078520-dbe8-44b1-9a08-bd2806690d79.png)

  - Dynamic Caching은 최초에는 Cache 서버에 콘텐츠가 없으나, 사용자가 콘텐츠를 요청하면 Cache 서버에 콘텐츠가 있는지 여부를 확인. 없으면 오리진(Origin) 서버에서 단운로드 받아 사용자에게 전달하고, 이후 동일 요청을 받게 되면 캐싱된 콘텐츠를 사용자에게 제공하게 됩니다. 콘텐츠는 일정시간(TTL)이 지나면 캐싱된 파일이 삭제될 수 있지만, 필요한 경우 다시 오리진 서버에서 콘텐츠를 확인 후 계속 가지고 있을 수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/159151022-9fdecfce-6f0f-4e15-9da2-2d305db2ea13.png)


<br>

- Amazon CloudFront

: 짧은 지연 시간과 빠른 전송 속도로 최종 사용자에게 데이터, 동영상, 애플리케이션 및 API를 안전하게 전송하는 글로벌 콘텐츠 전송 네트워크(CDN)서비스.

CloudFront는 AWS와 통합되며, 여기에서 AWS에는 AWS 글로벌 인프라와 직접 연결된 물리적 위치뿐만 아니라 DDoS(Distributed Denial of Services)와 같은 외부 공격을 완화하는 AWS Shiedld, 애플리케이션의 오리진 Amazon S3, 애플리케이션의 오리진으로의 Amazone EC2 또는 Elastic Load Balacing, 최종 사용자와 가까운위치이에서 사용자 정의 코드를 실행하도록 지원하는 Lambda@Edge 등의 서비스와 원활하게 연동되는 소프트웨어가 포함.

CloudFront는 선결제 금액이나 장기 약정 없이 사용량에 따라 지불하는 간편한 요금 모델을 제공, CloudFront에 대한 지원은 기존 AWS Support 구독에 포함되어 있습니다.

![image](https://user-images.githubusercontent.com/62640332/159151713-4ae11e15-d990-496c-ae02-560cab1798bd.png)

<br>

- CloudFront의 특징

1. CloudFront Global Edget 서비스 : 글로벌 사용자를 대상으로 보다 적은 지연 시간으로 콘텐츠를 제공하기 우해 42개국 84개 도시에서 216개 CDN PoP의 글로벌 네트워크 보유하고 있음. 전세계 모든 글로벌 CDN 서비스 밴더(Vender) 중 가장 빠르게 성장하고 있는 글로벌 CDN 서비스 제공자.

![image](https://user-images.githubusercontent.com/62640332/159152855-1808d606-af39-402f-bc6c-6e8371be93a0.png)

2. CloudFront 연결 가능한 오리진(Origins) 서비스 : CloudFront는 오리진으로 여러 AWS 리소스와 Custom 시스템 사용을 지원. S3 버킷, EC2 instance, ELB 또는 사용자 지정 오리진을 지정 가능.

![image](https://user-images.githubusercontent.com/62640332/159153119-894dfc83-6eea-4dc6-b09f-815ddd7de109.png)

3. CloudFront 콘텐츠 제공 방식. 

![image](https://user-images.githubusercontent.com/62640332/159153131-66082b19-3aac-4b55-a7a9-bf740ab902e2.png)

  3-1. 사용자가 웹 사이트 또는 애플리케이션에 엑세스하고 이미지 파일 및 HTML 파일 같은 하나 이상의 객체를 요청    
  3-2. DNS가 요청을 최적으로 서비스 할 수 있는 CloudFront 엣지 로케이션으로 요청을 라우팅합니다. 이 위치는 일반적으로 지연 시간과 관련해 가장 가까운 CloudFront 엣지 로케이션이며, 요청을 해당 위치로 라우팅합니다.      
  3-3. 엣지 로케이션에서 CloudFront는 해당 캐시에 요청된 파일이 있는지 확인. 파일이 캐시에 있으면 CloudFront는 파일을 사용자에게 반환합니다. 파일이 캐시에 없으면 다음을 수행.   

<br>

- CloudFront 주요 기능

1. 정적 콘텐츠에 대한 캐싱 서비스와 비디오 스트리밍 서비스 : CloudFront는 전세계를 대상으로 온디맨드 미디어 스트리밍 서비스 제공 가능. 온디맨드 스트리밍 서비스를 위해 CloudFront를 사용하면 MPEG DASH, Aplle HLS 등과 같은 일반적인 형식의 동영상 스트리밍 서비스 제공 가능. Amazon Elemental Media Convert와 같은 서비스를 사용하여 라이브 스트리밍 서비스 제공 가능.

![image](https://user-images.githubusercontent.com/62640332/159153225-8ef9d288-bb9f-4c8e-ab38-ad194d23a24a.png)

정적인 콘텐츠(이미지, CSS, HTML, Javascript 등)에 대해 전송 속도를 높일 수 있또록 Amazon 글로벌 백본 네트워크와 Edge 서버를 활용하여 해당 웹사이트에 방문하는 사용자에게 빠르고 안전한 환경 제공.

![image](https://user-images.githubusercontent.com/62640332/159153235-be655693-0e22-450f-96f0-e283ad3db636.png)

S3 버킷을 사용하여 정적 컨텐츠 서비스를 제공할 수 있으며, OAI(Origin Access ID)를 이용하여 콘텐츠에 대한 접근을 손쉽게 제한 가능.

2. 동적 컨텐츠에 대한 캐싱 서비스 : CloudFront는 웹 사이트의 전체 서비스에 해당하는 이미지, 동영상 등의 정적 파일 외에도 동적인 파일에 대해서도 Caching할 수 있다. 이중 빈번하게 갱신되거나 동적인 업데이트가 필요한 페이지나 콘텐츠에 대해서도 TTL을 설정하여 캐싱을 지원가능.

![image](https://user-images.githubusercontent.com/62640332/159153375-76f61100-4755-43ff-be90-48d4e6597be8.png)

3. 다양한 보안 서비스 : CloudFront를 사용하는 것만으로 DDoS 공격을 차단할 수 있습니다. 
  
  - Amazon Shield(Layer 3/4 단계 보호)를 통한 DDoS 공격 차단(무료)
  - Amazon CloudFront를 사용하면 기본으로 Amazon Shield를 사용하게 되며, 외부로부터 일반적인 공격 유형(Syn/UDP Floods, Reflection Attacks 등)에 대한 벙어 및 자동 탐지/대응 지원
  - Amazon WAF(Layer 7단계 보호)를 통해 웹 트래픽 모니터링 및 차단(유료)
  - AWS WAF는 CloudFront로부터 전달되는 HTTP/HTTPS 요청을 모니터링하여 웹 트래픽에 대해 모니터링과 사용자 정의에 따른 규칙을 지정하여 웹 트래픽을 차단할 수 있는 웹 애플리케이션 방화벽 서비스 제공
  - Sighned URL/Cookie를 통한 콘텐츠 보호
  - 인터넷을 통해 제공되는 콘텐츠에 대해 유료 사용자나, 특정 인증을 통과한 사용자에게만 콘텐츠를 제공하기 위해 AMazon Signed URL, Cookie를 통해 프라이빗 콘텐츠에 대해서만 안전하게 접근할 수 있도록 서비스 구성
  - HTTPS Redirections과 SSL 인증서 연동 서비스
  - CloudFront는 콘텐츠에 대한 보안을 위해 SSL을 통한 HTTPS 환경을 구성할 수 있으며, Customer의 SSL 인증서를 등록하거나, Amazon에서 제공하는 자체 SSL 서비스인 Amazon ACM(AWS Certificate Manager)과 연동하여 추가적인 SSL 인증서의 구매 비용 없이 무료로 제공하므로 ELB에서도 사용 가능하며, 등록/업데이트 갱신은 모두 AWS 내부에서 무료 제공

4. 비용 최적화를 통한 비용 절감 : S3 bucket, EC2 instance, ELB와 같은 서비스를 사용하게 되면 사용자에게 데이터를 전송하는데 필요한 Network Out 비용을 지불하게 됩니다. 다만 Amazon CloudFront를 사용하게 되면 기존 서비스에서 사용자에게 데이터를 전송시 지불되는 네트워크 Out에 대한 데이터 전송 비용을 지불하지 않으며, CloudFront 사용료에 대한 부분만 지불.

이렇게 오리진 Amazon 내에 있는 경우 네트워크 Out에 대한 비용을 지불하지 않게 되며, 기존 네트워크 Out보다 저렴하게 제공 가능. 이렇게 네트워크 사용료데 대한 최적화를 통해 비용 절감가능.

![image](https://user-images.githubusercontent.com/62640332/159159711-1bfc0326-1d8e-4632-bd61-6039bf62b526.png)


<br>
<br>

### ㅁ IAM(identity & Sccess Management)

: 통합 계정 관리를 지칭하는 용어. IAM은 일반적인 환경과 SOA(Services-Oriented Architecture) 및 웹 서비스 환경에서도 IT 관리에 대한 통합적인 관리방법을 제시하면서, ID 및 액세스 관리를 효과적으로 수행할 수 있도록 지원하는 솔루션. 지속적으로 증가하는 내/외부 사용자의 가용자 계정을 생성, 수정 및 삭제하는 작업을 자동화하고, 메인프레임에서 웹 애플리케이션에 이르는 모든 범위의 전사적 시스템 상에서 감사 기능을 제공해 고객들의 비지니스를 보다 안정적이고 효율적으로 지원.

<br>

- 계정 관리 시스템의 종류

![image](https://user-images.githubusercontent.com/62640332/159161495-0dd9cd76-4c17-4e0b-aea2-65efbccdcdd0.png)

<br>

- IAM 서비스

: IAM는 Amazon Web Services 리소스에 대한 액세스를 안전하게 관리할 수 있ㄴ게 해주는 서비스로 AWS 사용자 및 그룹을 만들고 관리하며, 권한을 사용해 AWS 리소스에 대한 액세스를 허용 및 거부할 수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/159161653-5c8d790c-64c9-4b31-8857-47572a3b0805.png)

암호나 액세스 키를 공유하지 안혹도 AWS 계정의 리소스를 관리하고 사용할 수 있는 권한을 다른 사라멩게 부여 가능.

리소스에 따라 여러 사람에게 권한을 부여하거나 특정 EC2 및 애플리케이션에서 실행 가능하도록 안전한 방법 제공.

또한 계정 보호를 위한 멀티 팩터인증(MFA)을 통해 사용자 계정 및 암호에 추가적인 인증을 통해 꼐정 보호 기능을 제공.

기업 네트워크나 인터넷 자격증명 공급자와의 연곌를 통해 이미 다른 곳에 암호가 이쓴ㄴ 사용자에게 AWS 계정에 대한 임시 액세스 권한 부여 가능

![image](https://user-images.githubusercontent.com/62640332/159161719-438b2e8f-c82d-4ed7-8918-ff53576148bc.png)

<br>


- IAM의 주요 특징

1. IAM 사용자와 그룹의 정의

  - IAM 사용자(User) : AWS에서 생성하는 개체로 AWS와 서비스 및 리소스와 상호 작용하기 위해 그 개체를 사용하는 사람 또는 서비스. IAM 사용자는 필요에 따라 신규 생성, 수정, 삭제할수 있으며, 하나의 AWS 계정에 최대 5,000 개의 계정을 생성할 수 있고, 각 AIM 사용자는 오직 한 개의 AWS 계정만 연결된다.    
   
  신규 생성된 IAM 사용자는 아무런 권한도 할당되지 않으며, AWS 작업을 수행하거나 AWS 리소스에 액세스할 수 있는 권한 X.

  이후 필요한 AWS 리소스에 접근하기 위해 사용자계정에 직접 권한을 할당하거나, IAM 그룹(Group), IAM 역할(Role)과 사용자 계정을 추가하여 권한 및 자격증명을 할당할 수 있다.

  또한 개별 IAM 사용자에게 권한을 할당하여 이들이 AWS 리소스를 관리하고 다른 IAM 사용자가까지 생성하고 관리하도록 구성 가능.

![image](https://user-images.githubusercontent.com/62640332/159161863-e37f5968-1d45-4f13-b9ff-6f5987fdde84.png)

  - IAM 그룹(Group) : IAM 사용자들의 집합. Admins라는 그룹을 만들어 관리자에게 필요한 유형의 권한을 부여하면, 그룹에 할당된 권한이 그룹에 속하는 모든 사용자에게 자동으로 부여

![image](https://user-images.githubusercontent.com/62640332/159161979-d766d13a-23d4-4d7a-983b-5a5a37ff7c1a.png)

  - IAM 역할(Role) : AWS에서 자격증명을 처리하거나 하지 못하도록 권한 및 정책을 보유하고 있다는 측면에서 IAM 역할(Role)은 IAM의 사용자와 유사. 하지만 IAM의 역할은 한 사용자만 연결되지 않고 그 역할이 필요한 사용자 또는 그룹이면 누구든지 연결할수 있도록 고안됨.

AWS 계정 사용자에게 리소스에 대한 액세스 권한을 부여하거나, 하나의 AWS 계정 사용자에게 다른 계정 리소스에 대한 액세스 권한을 부여해야하는 경우나 모바일 앱에서 AWS리소스를 사용할 수 있도록 하되 앱에 WS 키를 내장(교체하기 어렵고 사용자가 추출할 가능성이 있음)하길 원치 않는 경우도 있슴. 이러한 경우 IAM 역할을 사용하여 AWS 리소스에 대한 액세스 권한을 위임 가능.

![image](https://user-images.githubusercontent.com/62640332/159162072-688a8a6f-8ffb-4bd9-a70f-d0cf0f339b4a.png)


2. IAM 서비스의 동작 방식

: IAM은 AWS 내 리소스 및 자원 관리를 위한 사용자와 그룹의 생성 및 관리 기능을 통해 고유한 보유 자격증명을 생성할 수 있으며, AWS 서비스 API와 리소스에 대한 권한을 부여함으로써 AWS 서비스와 내부 리소스에 대해 강력한 보안을 통해 안전하게 리소스 관리 가능.

관리자 계정은 EC2에 대해 모든 작업이 가능한 권한 보유하고 있기 떄문에 EC2에 대해 정지(Stop), 종료(Terminate)가 가능. 하지만 EC2 권한이 없는 개발자 계정의 경우 EC2인스턴스에 대해 정지/종료가 불가능

![image](https://user-images.githubusercontent.com/62640332/159164459-fdf70ba1-e5bc-4454-9f89-5f5a0fdf6db9.png)

IAM은 사용자 계정을 통해 개별 AWS 리소스에 대한 세부적인 권한 부여 가능
  
  - AWS 리소스를 관리하기 위한 콘솔(Console)에 대한 접속 권한
  - AWS 내부 리소스에 대해 접속 권한
  - AWS 내 데이터에 대해 프로그래밍 방식(API)으로 접속하는데 필요한 권한


<br>

- IAM 자격증명 관리 기능  
 
![image](https://user-images.githubusercontent.com/62640332/159164524-bc634361-2cff-4466-b298-879974398d5d.png)


- 타 인증 시스템과의 연동 :  IAM의 Federation 서비스를 이요하면 AWS 리소스를 중앙에서 관리 가능. Federation과 함께 SSO(Single Sign-On)를 사용하여 회사 내에서 사용하는 LDAP나 Active Directory와 연ㄷ옹 가능하며, 이를 통해 AWS 계정에 액세스 가능.

![image](https://user-images.githubusercontent.com/62640332/159164587-0af6a0aa-191d-4314-a9f1-b0e1ab6e0f4f.png)

또한 Federation은 SAML(Security Assertion Markup Language 2.0)과 같은 개방형 표준 인증을 사용하여 ID 제공자(IdP)와 애플리케이션 간에 ID 및 암호를 교환한다.


 
![image](https://user-images.githubusercontent.com/62640332/159164629-3327b03d-abff-43f9-b57a-78f33a210058.png)


### ㅁ 호스팅(Hosting)

: 인터넷상에서 웹 서비스 제공을 위한 웹 사이트나 홈페이지를 운영하기 위해 필요하 ㄴ서버 장비, 인터넷 회선 등을 직접 구매하여 운영하지 않고, 서버를 임대하거나 웹 서비스에 필요한 웹(WWW) 공간을 입대(Hosting)하여 제공하는 서비스를 호스팅 서비스(Hosting Services)라고 합니다.

- 호스팅 서비스의 유형

1. 웹 호스팅 : 웹 서비스에 필요한 간단한 서비스를 저렴한 비용으로 사용할수 있는, 여러 대의 웹사이트르 한 서버에 공용 홍스팅으로 이요하는 서비스.

쉽게 관리 가능 하지만, 오버 셀링에 취약하며, 한사람이 과도한 리소스를 사용하면 다른 사람도 영향ㅇ르 받음.

<br>

2. 메일 호스팅 : 메일을 사용할 수 있는 계정을 임대한느 서비스. Gmail 처럼 메일 주소가 정해져 있는 곳과 달리 대부분 사용자가 도메인을 직접 구매하고 연결하여 이메일을 만드느 식으로 제공. 흔히 포털 사이트에 메일 계정을 제공받지만, 독립적인 도메인에 연결할 경우 메일 호스팅을 받게 됩니다.

메일 호스팅은 크게 웹 메일과 아웃룩 메일로 나눌수 있습니다.

메일 호스팅은 웹 사이트가 개설되기 전이라도 메일을 발급받을 수 있으며, 특히 메일을 독자적으로 구축하기에 부담이 되는 중소 규모 업체와 갠인이 이용하면 좋음.

<br>

3. 파일 서버 호스팅 : 흔히 HTML 코드 이미지 스크립트 프로그램 등이 웹 페이지에 소스로 뿌려지게 된다.

일반 웹 호스팅의 경우 OS 별로 다양한 프로그램 구현 등이 가능하지만 이미지 호스팅은 특정한 파일, 이미지만 서비스 가능하도록 제공하고 있습니다. 미디어 서버에 공간을 제공해 주는 스트리밍 호스팅과 유사하다고 할수 있으며, 이런 서비슨느 FTP는 제공되나 HTML, ASP, PHP, JSP 등 지원 X. 이미지에 대한 트래픽만 전문적으로 제공되므로 특정 페이지에 대한 트래픽을 감소하거나 프로모션등 특별한 목적이 생길 떄 전문적으로 이요하여, 활용 방법도 다양하다.

<br>

4. 서버 호스팅 : 호스팅 업체에서 제공하는 서버를 임대/구매하는 방식으로 한 대의 서버를 통쨰로 빌리는 방법. 홈페이지 방문자수가 많거나 용량이 아주많은 경우 생각해 볼수 있음. 그러나 서비스 비용 비쌈.

  - 전용 호스팅(Dedicated Hosting) => 서버 + 회선 임대 서비스 : 인터넷 서비스를 하기 위한 장비를 임대받아 서비스 하는 개념. 서버 임대는 서버 + 인터넷 회선

  일반 웹 호스팅은 서버 내의 일정한 공간 임대지만, 서버 임대는 서버를 독다적으로 운영 가능하며, 모든 권한을 준다. 사용량에 따라 회선 마음껏 조절 가능하며 서버 사양에 따라 가격이 각기 다르며, 소유권을 이전하냐 않하냐에 따라 차이 두기도 한다. 

  별도의 부가 서비스에 따라 다양하게 선 택 가능하며, 다수의 호스팅을 받고 있는 사용자가 서버 임대를 할 경우 가격이 절감할 수있으며 다양하게 운영 가능.

  공통으로 운영되고 있는 서비스에서 트래픽 발생이 높다면 서버 임대 쪽을 생각해 보는 것이 필요.

  
  - 코로케이션(Colocationm) => 회선 + 상면 임대 서비스 : 인터넷 서비스에 필요한 각종 장비를 인터넷망에 연결해야 하는데, 이를 독자적으로 구성하기에는 전원, 회선, 방재 등 다양한 응급 환경을 대처할 수준으로 서버들만이 편안한 휴시퍽라 하는 인터넷 데이터 센터에 입주 시켜 서비스 받는 형태.

  소규모 사이트를 이요하기보다는 특화되고 전문적이 형태의 서비스의 경우 코로케이션 서비스를 이요하게 됨. 

  고객이 인터넷 사이트나 서비스를 운영하는데 필요한 최적의 시설, 네트워크 접속, 관리 서비스를 제공하기도 함. 또한 DIC의 전용 공간, 인터넷 백본 연결, 관리 지원 서비스를 통해 자신의 서버와 네트워크 장비의 안정적인 운영을 보장 받기 가능



<br>
<br>


- 트래픽(Traffic) : 사이트 접속 시 방문자에게 전송되는 모든 데이터의 총량의미.

웹 페이지(텍스트, 이미지)를 보거나 음악, 동영상 파일 재생 또는 다운로드 할떄 발생. 용량 단위로 표현하지만 웹 공간(HDD)이나 데이터베이스 공간을 나타내는 물리적 용량과는 조금 다른 개념.

 
- 트래픽 제한 정책 : 웹 호스팅은 한 대의 서버를 여러 개의 공간으로 나누어 저렴한 비용으로 여러 명이 함께 사용하는 서비스. 따라서 사용자 간에 지켜야할 규범 있으며, 그중 트래픽사용량을 지키는 것이 우선 요건.

서버 한대에 할당된 트래픽을 공정하게 배운하여 서버를 공동 사용하는 모든 사이트가 원활히 서비스 될수 있도록 관리하는 것이 호스팅 업체의 의무, 웹 호스팅 고객역시 한 사이트의 과도한 트래픽이 타 사이트에 피해줄수 있다.

- 트래픽 초기화 : 일반적인 웹 호스팅의 트래픽은 매일 자정(0시)에 자동 갱신. 트래픽 초과로 사이트 접속이 차단되어도 자정이 지나면 자동으로 다시 열린다. 일일 허용 트래픽을 초과하지 않은 남은 용량이 있어도 다음 날 넘어가지 않고 초기화. 그러나 트래픽이 초기화 되기 전 약정된 일일 트래픽 전송량이 110%에 도달하면 사이트 차단.


<br>
<br>

- Amazon Lightsail : 간단한 가상화 프라이빗 서버(Virtual Private Server, VPS)가 필요한 개발자에게 웹 사이트와 웹 애플리케이션을 배포하고 관리하는 기능과 컴퓨팅, 스토리지, 네트워크를 제공.

Lightsail은 사용하기 쉬운 사용자 인터페이스를 갖추고 있으며, 비용이 효율적이고 빠르고 믿을 수 있는 가상 사설 호스팅 서비스 제공.

![image](https://user-images.githubusercontent.com/62640332/159168610-f3f39794-0ec2-4cc3-83b8-87b4bc92bb68.png)

<br>

- Lightsail 특징 : Lightsail은 프로젝트를 빠르게 시작하는데 필요한 모든 서비스(가상 머신, SSD 기반 스토리지, 데이터 전송, DNS관리, 고정 IP)를 포함하고 있습니다.

일반적인 경우 EC2 생성을 위해 여러 스텝과 절차를 거치지만, Lightsail 사용하면 손쉽게 서버 생성 가능.

 
![image](https://user-images.githubusercontent.com/62640332/159168666-27ec439b-976c-48e8-9484-25b86e420f7b.png)

Lightsial은 몇몇 가상 프라이빗 서버와 간단한 관리 인터페이스를 선호하는 개발자에게 가장 적합한 서비스. 소프트웨어나 프레임워크를 설치하는 시간을 줄일 수 있도록 다양한 인스턴스 이미지 제공.

![image](https://user-images.githubusercontent.com/62640332/159168704-f787c768-48bf-40e5-9b16-4d43cdd1b954.png)

  - 서비스에 따라 사용량 증가시 SSD 기반 블록 스토리지 추가 하고, Lightsail인스턴스와 연결 가능.
  - 인스턴스와 디스크의 스냅샷을 통해 새로운 인스턴스 손쉽게 만들기 가능
  - Lightsail 인스턴스가 외부의 다른 AWS 계정과 VPC Peering을 통해 AWS의 다른 리소스 와 영ㄴ결하고 서비스 확장 가능.
  - Lightsail 로드 밸런서 생성하고 인스턴스를 연결하여 고가용성 애플리케이션을 구성가능
  - 암호화된(HTTPS) 트래픽, 세션 지속성, 상태 확인, 안정성과 가용성 높은 서비스 제공 가능.

<br>

- 사용 가능한 리전 및 가용 영역 : 13개의 글로벌 리전과 38개의 가용 영역을 통해 휍 사이트 및 앱이 필요한 곳에 Lightsail 서버 생성 가능.

가용 영역은 물리적을 구분된 자체 독립 인프라에서 운영되는 데이터 센터를 모은 것으로 높은 안정성을 갖추도록 설계.

발전기 및 냉각 장비 등에 발생하는 일반적인 장애 사항은 가용 영역 간에 공유되지 X.

화재, 토네이도, 홍수 등 극한의 재해 상황 발생해도 단 하나의 가용 영역에만 영향을 미치게 된다.


<br>

- Lightsail 인스턴스 이미지 : 소프트웨어나 프레임워크를 설치하는 시간 줄이기 위해 다양한 인스턴스 이미지 제공.

 
![image](https://user-images.githubusercontent.com/62640332/159168900-bc34b58f-3e66-40b9-ae49-1a6fc8350404.png)

<br>

- Lightsail 요금제

![image](https://user-images.githubusercontent.com/62640332/159168915-e77b4555-48e5-46c3-b5b1-746ecc5fca15.png)


![image](https://user-images.githubusercontent.com/62640332/159168936-595c5cee-1b7b-4291-a80f-bea95be6aa63.png)

자세한 사항은 여기 참조] https://aws.amazone.com/ko/lightsail/faq/

<br>

- 애플리케이션 확장성과 고가용성 지원: Lightsail의 블록 스토리지 추가, 로드 밸런스 구성, AWS Services와의 연계를 통해 고가용성(High Availability) 애플리케이션을 손쉽게 구성가능.

![image](https://user-images.githubusercontent.com/62640332/159168981-16f6dd00-2dd5-425a-902e-ac5016670ec3.png)