
---

### 클라우드 용어

ㅁ 영역

- 리전(Region) : 물리적으로 위치가 다른 나라에 서버 설치
- 가용 영역(Availability Zone) : 데이터 센터(IDC; Internet Data Center)의미하며, 하나의 리전에 다수의 가용 영역(AZ) 보유
- 엣지 로케이션(Edge Location) : CDN 서ㅣ스인 CloudFront를 위한 캐시 서버(Cache Server)emfdml ahdma


ㅁ EC2(Elastic Compute Cloud)

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

ㅁ EBS(Elastic Block Storage)

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


ㅁ 보안 그룹(Security Group)
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

ㅁ 스토리지(Stroage)

1. DAS(Direct Attatched Stroage) : 서버에 직접 연결하는 방식
2. NSA(Network Attached Storage), SAN(Storage Area Network) : 스토리지를 빠른 속도의 네트워크로 연결하는 방식    
 
- NSA 는 LAN(Local Area Network)을 연결하여 사용하기 때문에 비용 저렴
- SAN 는 확장 용이, 대규모 엔터프라이즈 환경에 적합, 고속의 전용 네트워크를 구성하여 빠른 속도의 스토리지 서비스 제공

NAS 와 SAN 의 차이점은  SAN은 블록 수준에 데이터 저장, NSA는 파일 단위로 데이터 접속    
OS 입장에서 보면 SAN은 일반적으로 디스크로 나타나며 별도로 구성된 스토리지용 네트워크 존재, 반면 NAS는 OS에 파일 서버로 표시.


ㅁ S3(Simple Storage Services)
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
4. Glacier
