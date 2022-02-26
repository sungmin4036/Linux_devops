![image](https://user-images.githubusercontent.com/62640332/155832410-6d101ad6-7736-4391-95db-9284319d3d0a.png)


ㅁ 주요 고려사항

- 서버 선정 및 이중화 구성
- DB 구성
- 사용자 인증 서비스
- 소스코드 관리 및 배포

ㅁ 용도에 맞는 EC2 서버 선택

- 업무에 적합한 서버 선정
  - CPU, 메모리, I/O 별 인스턴스 타입
  - 필요 성능에 따른 스토리지 선택



![image](https://user-images.githubusercontent.com/62640332/155832442-3f08fd23-e3dc-411d-acfa-fca6650927ae.png)

![image](https://user-images.githubusercontent.com/62640332/155832450-84a1e544-a9a2-4fd9-8241-a207850a12d5.png)


서버를 이중화 구성시, 하나의 가용 영역에 이중화를 한경우에는 가용영역 장애 발생시 서비스 전체제  서비스 불가상태가됩니다.

=> AWS multiage의 이점을 살려서 리전 내에 서로다른 가용영역에 이중화 구성을 하게되면 더 높은 가용성을 확보 가능합니다.

![image](https://user-images.githubusercontent.com/62640332/155832490-7b64e299-6169-43dc-a66a-5c6bc413706a.png)

![image](https://user-images.githubusercontent.com/62640332/155832506-7eef28d8-3f0c-44a8-8c2a-61885fb252b9.png)

ALB는 L7 로드밸런서로 url path, http 메쏘드 유형, http 헤더 등을 기준으로 요청을 목적지 서버로 분산합니다.

NLB는 L4 로드밸러서로 포트값을 기준으로 요청을 분산합니다.

=> NLB는 로드밸런서가 IP로 고정되기 떄문에 도메인명으로 통신할수없는 워크로드의 경우 유용하게 사용 가능합니다.

모두 고가용성과 자동확장 기능 제공하고있습니다.

![image](https://user-images.githubusercontent.com/62640332/155832562-c2780994-4d32-44e3-9154-dba36aad3429.png)

사용자가 DB 직접설치 필요X, 다양한 기능 자동화 가능

![image](https://user-images.githubusercontent.com/62640332/155832587-7f20e8c2-0ff6-4fa4-bebb-76f60d594817.png)

3개의 가용영역에 6벌 복제하여 안정적으로 운영 가능

![image](https://user-images.githubusercontent.com/62640332/155832609-f5d0a32a-c29c-4806-9417-9502c3730737.png)

사용자가 브라우저에서 요청 전송시, 라우터 53 에서 도메인에 해당하는 ip를 얻어와서 ELB로 요청전송, 요청이 어플리케이션 서버 2중하나로 전송되면

로직이 실행되면서 RDS로 구성한 DB에서 sql 작업을 수행하게 됩니다.


<br>
<br>
<br>

사용자 인서 서비스 구현

![image](https://user-images.githubusercontent.com/62640332/155832647-ca6d9bf6-84bf-412f-a94a-a4beacb29a6d.png)

구글, 페이스북 과같은 로그인도 지원

![image](https://user-images.githubusercontent.com/62640332/155832665-8fd1f3b9-7bf8-4bc3-b601-a6ca655e8f6a.png)

aws는 codepipeline을 제공하며 github, jenkins 와같은 써드파티 제품과 같이 사용 가능합니다.

<br>
<br>
<br>
<br>

ㅁ 사용자 > 1,000 

- 주요 고려사항
  - 시스템 확장
  - DBMS 고 가용성
  - 백업
  - 시스템 모니터링


![image](https://user-images.githubusercontent.com/62640332/155832730-f708f662-9ac6-4b05-a77f-4c6842ceccf7.png)

사용자가 많아지면 특정시간대에 몰리는 경우가 발생=> 서버 대수가 탄력적으로 증가 되어야 한다.  ==> Auto scaling 사용

ex) 서버들의 평균 cpu 사용률이 70퍼 이상이되면 서버를 늘리고, 30퍼 이하되면 서버를 줄이는등을 설정해놔서 자동으로 조절

![image](https://user-images.githubusercontent.com/62640332/155832786-3385ba9e-b586-40be-bac9-93a1737f8d1e.png)

오토스켈링 적용 대상들을 하나의 오토스켈링 그룹으로 구성하면, EC2가 생성되면서 ALB가 자동으로  대상서버 적용시킨다.

신규로 추가된 서버가 서비스가능 서비스로 판단하면, ALB가 해당서버가 요청 밸런싱 하게 된다.


![image](https://user-images.githubusercontent.com/62640332/155832861-76ce41b2-2cfc-478d-84ac-8f19b2d58b1f.png)

![image](https://user-images.githubusercontent.com/62640332/155832885-14093c28-a416-4f61-93bc-7ad2323fdabe.png)

아마존은 DB 고가용성을 위해서 다중 AZ 배포 지원

다중 AZ 배포 적용해서 RDS를 생성하면 한 가용영역에 MASTER 인스턴스가 생성되고,

다른 가용영역에 stand-by 인스턴스가 생성됩니다. MASTER 인스턴스에서 변경된 데이터는 stand-by인스턴트에

동기식으로 자동 복제가 되기때문에, 항상 데이터 동기화가 이루어 집니다.

마스터 인스턴스나 가용영역 자체에 장애가생겨 서비스가 안되게 되면, stand-by 서버가 마스터 서버로 자동으로 승격됩니다.



 
![image](https://user-images.githubusercontent.com/62640332/155833041-7eeedca4-70fd-444c-bb8f-8790c77a7f5f.png)


![image](https://user-images.githubusercontent.com/62640332/155833043-67e43166-3e91-4749-9b69-86d32b60a130.png)

![image](https://user-images.githubusercontent.com/62640332/155833051-aeaa9a0a-daac-466f-bada-3eb7290f6346.png)

마스터 장애 발생시 stand-by 인스턴스가 자동으로 마스터 인스터스로 변경되고, RDS에 대한 DNS 정보가 업데이트 됩니다.

이경우 데이터가 동기화 되있어서 데이터 유실없이 RDS가 다시 서비스 가능 상태가 됩니다.

![image](https://user-images.githubusercontent.com/62640332/155833109-f8c5ea54-a74f-4cfd-a4dd-f5a998fc7e43.png)

어플리케이션이 DNS lookup으로 새로운 RDS 마스터 인스턴스의 ip 감지하게되면, 새로운 마스터 DB에 접속해서 동작하게 됩니다.

![image](https://user-images.githubusercontent.com/62640332/155833140-d39afc8f-1b98-4bde-b7a7-94ec086de013.png)

최종 고 가용성 아키텍처 형태는 이렇게 됩니다. 

![image](https://user-images.githubusercontent.com/62640332/155833162-948b875a-8600-450c-931e-5a0e5d2d5bd5.png)

aws의 스토리지 서비스나 db 서비스에서도 백업을 지원하지만, aws backup서비스를 이용하면

데이터를 중앙집중화 하고, 자동화 가능해집니다. 백업 일정 및 수명관리 정책하면 자동으로 백업 수행 합니다.

aws storage gateway를 같이 사용하면 온프로미스 데이터를 aws로 백업도 가능해집니다.

보안을위해 백업데이터 암호화 및 접근통제도 가능해집니다.

![image](https://user-images.githubusercontent.com/62640332/155833222-c22114ab-62fa-4b92-a431-8d843f673e72.png)

특정 조건 발생시 알람을 발생하고, 그것에대한 조치도 자동으로 할수 있습니다.

EC2 auto scaling의 경우도 cloud watch의 기능을 이용하여 동작하게 됩니다.

cloud watch로 대시보드를 구성해서, 여러 리전에 구성된 리소스나 어플리케이션에 대해서 단일 모니터링도 가능합니다.

<br>
<br>
<br>
<br>

ㅁ 사용자 > 10,000

ㅁ 주요 고려사항

- 선능
- 부하분산

![image](https://user-images.githubusercontent.com/62640332/155833288-16a32acc-7be5-4ab3-80a7-1483e9cfbe79.png)

cloudFront는 aws의 CDN 서비스로, 사용자가 요청 전송시 해당 요청이 사용자와 가까운 cloudfront pop으로 먼저전달 됩니다.

한번 응답한 컨텐츠를 캐시에 뒀다가, 다음번에 동일 리소스 요청시 캐시해둔 데이터로 직접 응답을줘서 사용자에게 빠르게 응답을 해주며, 오리진 서버에 부하를 줄여줍니다.

그리고, 앤드유저에 latency 를 줄여 user experience 를 개선합니다.

![image](https://user-images.githubusercontent.com/62640332/155833365-9e42acb2-fda9-4876-9052-ea929def1b78.png)

클라우드 프론트를 적용하면 앤드유저가 시스템 접속시에 자신의 위치와 가까운 팝에 접속해서 캐시된 컨텐츠를 찾고

없을 경우 오리진 서버에 요청이 전달됩니다. 어플리케이션이 로직을 수행하고,  DB에서 데이터를 조회해 와야하는 동적 컨텐츠 요청의 경우에도 cloudfront를 이용하면 aws backbone network로 데이터가 전송되기 때문에 latency가 줄어듭니다.

![image](https://user-images.githubusercontent.com/62640332/155833447-fee4af07-c480-4605-ac3b-44fa21cc4cff.png)

시스템 성능개선이나, 부하분산을 위해서 DB의 부하를 줄이는것도 중요한 방법중 하나

마스터 DB 부하 줄이기 위해서 select문을 처리하는 읽기전용 인스턴스를 구성하는경우가 많습니다.

aws rds는 이러한 읽기복제본을 쉽게 구성할수있는 기능 제공합니다.

특히 오로라는 읽기복제본에 auto-scaling을 적용해서 평균 cpu 사용률이라던지, 평균 connection수 등을 기반으로

최대 15대 까지 auto-scaling을 구성해서 부하에 유연하게 대응하게 하는 기능 제공합니다.

읽기복제본이 많더라도 단일 end-poing 제공하기 떄문에 에플리케이션에서 부하분산을 위해서 별도로 고민 필요 X

![image](https://user-images.githubusercontent.com/62640332/155833534-73e01668-7421-470c-8d02-a9077473418c.png)

어플리케이션에서 CUD쿼리는 마스터 인스턴스로 전송하게하고, select 쿼리는 읽기복제본으로 전송되도록 코딩해서

부하분산 가능해집니다. 마스터 인스턴스의 변경되 데이터는 읽기보제본 인스턴스에 비동기 방식으로 복제 됩니다.

만약 어플리케이션 로직에서 엄격한 정합성 필요시, 해당 select 퀄리 마스터 인스턴스에서 수행되도록 구현 가능합니다.

<br>
<br>
<br>

ㅁ 빈번하게 access해야하는 데이터의 경우에는 DB 보다는 Cache 서버를 구성해서 더 빠르게 응답 가능

EC2에 auto-scaling 적용한 경우에는 http 세션과 같은 데이터를 개별 ec2 메모리에 저장하는거 보다는

별도로 Cache 서버를 두고, 중앙에서 관리하는것이 더 유용합니다.

![image](https://user-images.githubusercontent.com/62640332/155833655-9f7b6182-eace-4d7a-aaae-7045eab7da2f.png)

aws elastiCache 서비스는 쉽게 캐시 클러스터를 구성할수 있도로 제공하는 관리형 서비스 입니다.

오픈소스 Memcached 와 redis를 지원합니다. 

![image](https://user-images.githubusercontent.com/62640332/155833705-f8041946-11a6-408b-b64b-e5a49eaebec1.png)

redis용 elastiCache를 적용한 아키텍처로, ElastiCache서버를 마스터 서버 외에 읽기전용복제본을 구성하면 부하에 더 대응가능합니다.

또 다중 에이지 기능을 이용하면 마스터 서버 장애발생시 읽기전용복제본 서버가 자동으로 마스터로 승급되어서

자동으로 읽기,쓰기 다시 지원하게 되어 시스템 고가용성 확보 가능해집니다.

<br>
<br>
<br>

ㅁ 사용자 > 100,000

ㅁ 주요 고려사항
    - Loosely Coupled, 분산 아키텍처
    - 서버리스 아키텍처
    - 분산 시스템 모니터링

사용자가 증가하더라도, 서버관리에 대해 고민필요없는 서버리스 아키텍처 사용 좋다.

![image](https://user-images.githubusercontent.com/62640332/155833831-3d170ed4-1b9a-48a6-98f3-a11fd75cd0db.png)

서비스간의 결합도를 낮추는 아키텍처로 대표적인것이 queue 와 Topic 방식으로 메세지를 전달하는것입니다.

메세지를 처리하는 consumer에 상관없이 프로듀서가 메세지를 큐나 토픽에 넣기만하면 되기때문에

loosely coupled 시스템 구축시에 많이 사용됩니다. 

SQS 는 프로듀서가 메세지를 큐에 넣으면, 컨슈머가 꺼내서 사용하는 pull 방식의 서비스

SNS 는 프로듀서가 토픽에 메세지를 개시하면, 해당 토픽을 구독하는 여러 대상으로 SNS가 푸시 방식하는 서비스

이 서비스들은 워크로드 부하에따른 자동으로 확장되기 때문에 관리 오버해드가 없습니다.

![image](https://user-images.githubusercontent.com/62640332/155833939-9215e951-ac20-44e0-9a9b-6dfe1bd8f8a7.png)


![image](https://user-images.githubusercontent.com/62640332/155833972-691e8117-f429-4065-87b8-cf03674e5aa8.png)

서버리스 아키텍처는 사용자가 서버를 미리 프로비저닝 해두거나 관리필요없이 어플리케이션 로직만 개발하면

요청에 따라서 탄력적으로 리소스가 스케일링 되는 컴퓨팅 서비스 입니다.

대표적인것이 AWS Lambda

특정 이벤트 발생시 Lambda가 실행되도록 가능합니다.

ex) s3에 이미지 파일 업로드 되면 이미지 포맷, 이미지 크기변경하는 람다 프로그램 호출되거나,

ex) dynamo db table, kinesis, SQS 등의 데이터 반영시 후속작업 처리하는 람다 프로그램 호출 되는등 가능 여러 개발언어 지원


![image](https://user-images.githubusercontent.com/62640332/155834020-6974a63a-f13a-4fe2-a682-94e44361d4db.png)

aws 는 여러 서비스 통합에 lambda를 활용하며, 다른 여러 서비스들이 lambda를 trigering 하면서 데이터를 넘겨주기 때문에

데이터 꺼내는 과정 필요없이 비지니스 로직만 개발하면됩니다.

그리고 요청 증가시 lambda container가 자동으로 기동되어, 동시처리량 늘리고, 요청 줄어들면 컨테이너가 자동으로 종료 되어 관리부담 내려가고, 가격도 효율적으로 관리 가능합니다.

![image](https://user-images.githubusercontent.com/62640332/155834072-dfaa4e3c-1c0c-40be-9071-909a8b6a58b3.png)

시스템 규모가 커지면 ,DB도 용도에 맞게 사용해야합니다.

많은 기업들이 OLTP성 데이터 처리에 RDMBS를 사용하고, OLAP성 데이터 처리에는 DataWarehouse를 사용합니다.

그리고, 고정된 형태의 스키마구조가 아니라 key, value, document 형식으로 데이터 빠르게 처리할 경우 nosql 사용중입니다.

RDS - RDBMS
Redshift - DW서비스
ElasticCache - cache 나 In-memory로 사용
DynamoDB, DocumentDB, Managed Apache Cassandra - nosql 서비스
Neptune - 데이터 상관관계 관리하는 Graph DB
timestream - serial 데이터를 저장하고 빠르게 쿼리 가능
QLDB - 변경 불가능한 원장데이터를 중앙에서 관리

```
1) OLTP: On-Line Transaction Processing (데이터 갱신위주)

: 네트워크 상의 여러 이용자가 실시간으로 데이터베이스의 데이터를 갱신하거나 조회하는 등의 단위작업을 처리하는 방식을 말한다.

2) OLAP: On-Line Analytic Processing (데이터 조회위주)

:정보위주의 처리 분석을 의미한다. 의사결정에 활용할 수 있는  정보를 얻을 수 있게 해주는 기술이다.

OLTP에서 처리된 트랜잭션 데이터를 분석해 제품 판매추이, 구매성향파악, 재무회계분석 등을 프로세싱하는 것.
```

![image](https://user-images.githubusercontent.com/62640332/155834285-0f250759-f7ab-4033-ae06-f2223c3c3615.png)

좌측은 업무로직중 특정 데이터를 data warehouse에 넣고 분석해야할경우 업무A 서버에서 SQS로 메세지를 Put하면

업무 B서버 에서 메세지를 꺼내서 RedShift로 적재하는 방식으로 구성한 모습입니다.

우측은 S3에 파일이 업로드되면, Lambda에서 parsing해서 DynamoDB에 데이터를 넣고 

사용자가 DynamoDB의 데이터를 조회해야 할경우 API Gateway로 요청 보내서 Lambda에서 필요한 로직 수행하고

DynamoDB에서 데이터 조회해서 응답하는 아키텍처

모든것을 하나의 서버에서 처리하는 방식이 아니라 분산해서 처리하고 있습니다.


![image](https://user-images.githubusercontent.com/62640332/155834371-a93713b7-d6f5-40bc-939b-36309ddc60fe.png)

시스템이 분산되면서 이에대한 모니터링이 필요해집니다.

X-Ray는 분산 애플리케이션의 분석이나 디버깅을 위한 서비스로 요청이 어플리케이션을 통과하는 전체 과정을

추적해서 서비스 맵을 제공합니다. 사용자는 수집된 데이터로 어느위치에서 성능 병목이 있는지 어디서 문제가 발생하는지 쉽게 추적 가능합니다.


![image](https://user-images.githubusercontent.com/62640332/155834441-0235b054-ecd4-4b58-9152-ed5cdc7ac960.png)


<br>
<br>
<br>
<br>

ㅁ 사용자 > 1,000,000

ㅁ 주요 고려사항
    - 재해복구(DR)/ 멀티 리전 시스템 고려
    - 멀티 리전 시스템 배포

![image](https://user-images.githubusercontent.com/62640332/155834464-b7517429-4674-473f-b648-a89b0f9cac73.png)


시스템 규모가 커지고 복잡해지면 관리 복잡도도 증가합니다. 다른 리전에 동일한 시스템을 추가로 구성하거나

아키텍처가 변경될때마다 개발 테스트 운영시스템을 일일리 관리하는것은 점점 힘들어집니다.

이런경우 cloudFormation을 활용하여 infra structure를 코드로 관리하는것이 좋습니다.

생성해야되는 리소스등을 YAML 이나 JSNO 파일로 작성후 이 파일을 업로드하여 cloudformation 수행하면

설정한대로 리소스가 생성됩니다. cloudformation stack을 업데이트 해서 이전의 클라우드 포메이션으로 생성했던

리소스들을 변경할수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/155834573-a8a4d293-f8e1-4795-a716-6450f9b43266.png)


다른 리전으로 시스템 확장시 인프라 뿐만 아니라 데이터도 다른 리전으로 잘 전송할수 있어야 합니다.

오브젝트 스토리지서비스인 S3는 한 리전에 버킷에 파일이 반영되면, 다른 리전의 버킷에 자동으로 복제되도록 가능

파일에 태그를 설정하여 태그기반으로 복제돈 파일을 필터링도 가능합니다.

S3 교차리전복제 기능 사용하면 파일 동기화 위해서 별도로 시스템 구축필요없이 자동으로 데이터 동기화 실행 가능

![image](https://user-images.githubusercontent.com/62640332/155834667-789615a4-a9d9-4b4d-a1ea-491854438c1d.png)

RDS또한 다른리전으로 데이터 복제 가능합니다.

RDS는 동일 리전 S3에 snapshot으로 복제가 되는데, 이 snapshot은 테이블 데이터 뿐만 아니라 DB 엔진까지 전체를 백업하기 때문에

snapshot을 다른 리전으로 카피해서 그곳에서 다시 DB 기동 가능합니다.

![image](https://user-images.githubusercontent.com/62640332/155834708-0fb79b17-b483-44e9-abc2-391aa60e3545.png)

이 기능은 DynamoDB에서도 적용 가능합니다.

dynamoDB에서 원하는 테이블을 백업 받아서 다른 리전으로 복사한뒤 해당리전에서 dynamoDB를 복원할수 있습니다.

이경우에 특정 시점으로 복원하는 Point-In-Time Recovery 기능도 지원합니다.


![image](https://user-images.githubusercontent.com/62640332/155834747-94d72044-e06a-4fd7-b89b-0e558d24e8ed.png)

redis에서도 가능하며 한 리전에서 데이터를 내보내고, RDB 파일을 다른 리전으로 복사한후 해당 RDS파일을 사용해서 새 eleastic Cache cluster를 생성 가능합니다.

![image](https://user-images.githubusercontent.com/62640332/155834762-a12e1a5d-8b99-49cb-bcf1-594f7bb65f33.png)

멀티리전 시스템 구축했다면, 어플리케이션도 멀티리전에 배포가 되야 합니다.

aws codepipeline과 code deploy를 이용하면 배포 프로세서에 배포대상 리전을 여러개 추가하여 여러 리전으로 배포를 수행 가능합니다.

![image](https://user-images.githubusercontent.com/62640332/155834787-a9622f39-9e48-4dd5-96c9-c1d272fc55a6.png)

사용자 요청을 다른 리전으로 분기해야하는 경우에는 route 53를 이용하여 멀티리전 라우팅 구성 가능합니다.

한 리전이 서비스 불가시 다른 리전으로 장애 조치 라우팅 하거나 사용자의 지리적위치와 가까운 리전으로 라우팅 하거나

특정 비율로 라우팅 하는등 다양한 라우팅 정책 적용 가능합니다.

<br>
<br>
<br>

ㅁ 사용자수 > 10,000,000

ㅁ 주요 고려사항
   - 글로벌 시스템 확장
   - 멀티 리전 서비스 확대

![image](https://user-images.githubusercontent.com/62640332/155834853-4a2bfd90-3602-41d1-9317-0946c4927f74.png)

사용자가 너무 많아져서 RDS 서비스를 active-active가 필요한경우는 위에 말한 방법과는 다른 방법이 필요합니다.

RDS 오로라에 글로벌 DB를 사용하면 다른 리전으로 5개까지 데이터 복제가능합니다.

데이터 정합성을 위해서 CUD쿼리를 기본 리전에서 수행하면, 데이터를 1초 이내에 다른 리전에 있는 오로라 클러스터에 복제하기 떄문에

셀렉트 쿼리의 경우에는 각 리전에 오로라 인스턴스에서 수행하게해서 부하를 전세계 분포 시킬수 있습니다.

읽기, 쓰기가 수행되는 기본 리전의 장애 발생시에는 다른 리전에 오로라 클러스터를 읽기,쓰기가 가능하도록 변경하여 장애 대응 가능

![image](https://user-images.githubusercontent.com/62640332/155834961-f27dfe6e-b341-4a28-9f3d-5332eec89aea.png)

DynamoDB 의 경우 글로벌 테이블을 지원하며, 원하는 테이블을 글로벌 테이블로 구성하게되면

각 리전별로 로컬에서 읽기 쓰기가 가능해집니다. 그리고 변경된 데이터는 다른 리전들로 변경되어 동기화 됩니다.

ex) 아마존 프라임 비디오 서비스는 오라클을 dynamo global table로 이관하여 확장성, 성능, 비용절감 사용중


![image](https://user-images.githubusercontent.com/62640332/155835019-6552c2ca-4e9c-40ee-9ffe-851ddbe3b71f.png)

redis의 글로벌 데이터스토어 사용하면, 두개 리전으로 데이터 복제 가능합니다.

이러한 경우 elasticCache의 데이터 변경은 기본 리전에서 수행해야하지만, 변경된 데이터는 1초 이내에 다른리전을 복제되기 떄문에 멀티리전 서비스 이용 가능합니다.

기본 리전 장애시에 다른리전에 elastiCache를 읽기,쓰기 가능하게 하여 장애 대응



<br>
<br>
<br>
<br>

Quick Review

1. 멀티-AZ 활용하여 이중화 구성
2. EC2 오토 스케일링 이용
3. 성능 및 부하 분산을 위한 데이터를 캐시
4. 적절한 지표/모니터링/로깅을 하고 있는지 확인
5. 확장성이 뛰어난 관리형 서비스 활용
6. 클라우드포메이션 이용하여 프로비저닝
7. 용도에 적합한 DB사용
8. 느슨한 결함/분산 아키텝처 적용, 어플리 케이션 구조 변경
9. 글로벌 인프라스트럭쳐 활용하여 멀티리전으로 진화