##### 3 계층 아키텍처 개요
3 계층 아키텍처는 가장 많이 사용되는 멀티 계층 아키텍처이며 단일 프레젠테이션 계층, 논리 계층 및 데이터 계층으로 구성됩니다. 그림 1은 단순하고 일반적인 3 계층 응용 프로그램의 예를 보여줍니다.

![image](https://user-images.githubusercontent.com/62640332/156174455-bb6c2cc0-2b35-4077-b711-b097e975c824.png)

일반적인 3 계층 아키텍처 패턴에 대해 자세히 배울 수 있는 많은 유용한 자료가 온라인에 있습니다. 이 백서는 Amazon API Gateway 및 AWS Lambda를 사용하는 이 아키텍처의 특정 구현 패턴에 중점을 둡니다.

ㅁAWS Lambda:

- 선택, 보안, 패치 또는 관리 할 운영 체제가 없습니다.
- 적절한 크기, 모니터링 또는 확장 서버가 없습니다.
- 초과 프로비저닝으로 인한 비용 위험 감소
- 언더 프로비저닝으로 인한 성능 위험 감소

ㅁ Amazon API Gateway:

- API 배포, 모니터링 및 보안을 위한 단순화 된 메커니즘.
- 캐싱 및 컨텐츠 전달을 통한 API 성능 향상


##### API Gateway
Amazon API Gateway REST API는 리소스와 메소드로 구성됩니다. 

리소스는 앱이 리소스 경로 (예 : / tickets)를 통해 액세스 할 수있는 논리적 개체입니다. 

메소드는 API 리소스 (예 : GET / 티켓)에 제출 된 REST API 요청에 해당합니다. 

API Gateway를 사용하면 Lambda 함수를 사용하여 각 메소드를 백업 할 수 있습니다. 

즉, API Gateway에 노출 된 HTTPS 엔드 포인트를 통해 API를 호출하면 API Gateway가 Lambda 함수를 호출합니다.

프록시 통합 및 비 프록시 통합을 사용하여 API Gateway 및 Lambda 기능을 연결할 수 있습니다.

<br>
<br>

##### Amazon CloudWatch

Amazon API Gateway는 Amazon CloudWatch와 완벽하게 통합되어 API Gateway에서 원시 데이터를 수집하여 API 실행 모니터링을 위한 거의 실시간으로 읽을 수 있는 실시간 지표로 처리합니다. 

API Gateway는 구성 가능한 보고서 및 디버깅을 위한 AWS X-Ray 추적을 통한 액세스 로깅도 지원합니다. 

이러한 각 기능을 작성하려면 코드를 작성하지 않아도 되며 핵심 비즈니스 로직에 대한 위험 없이 프로덕션 환경에서 실행되는 응용 프로그램에서 조정할 수 있습니다.

<br>
<br>

##### API 인증

API의 일부로 생성 한 각 리소스 / 메소드 조합에는 AWS IAM 정책에서 참조 할 수 있는 고유한 특정 Amazon 리소스 이름 (ARN)이 부여됩니다.
Amazon API Gateway에서 API에 권한을 추가하는 일반적인 방법에는 세 가지가 있습니다.

- IAM 역할 및 정책 : 클라이언트는 API 액세스에 AWS Signature Version 4 (SigV4) 권한 부여 및 IAM 정책을 사용합니다. 

동일한 자격 증명으로 필요에 따라 다른 AWS 서비스 및 리소스 (예 : Amazon S3 버킷 또는 Amazon DynamoDB 테이블)에 대한 액세스를 제한하거나 허용 할 수 있습니다.

- Amazon Cognito 사용자 풀 : 클라이언트는 Amazon Cognito 사용자 풀을 통해 로그인하고 요청의 Authorization 헤더에 포함 된 토큰을 얻습니다.

- Custom Authorizer: 베어러 토큰 전략 (예 : OAuth, SAML)을 사용하거나 요청 매개 변수를 사용하여 사용자를 식별하는 사용자 정의 권한 부여 체계를 구현하는 Lambda 함수를 정의하십시오.

<br>
<br>

##### Amazon CloudFront

각 Amazon API Gateway 배포에는 기본적으로 Amazon CloudFront 배포가 포함됩니다. 

`Amazon CloudFront`는 Amazon의 엣지 로케이션 네트워크를 API를 사용하는 클라이언트의 연결 지점으로 사용하는 콘텐츠 전송 서비스입니다. 

이를 통해 API의 응답 대기 시간을 줄일 수 있습니다. 

Amazon CloudFront는 전 세계에서 여러 개의 엣지 로케이션을 사용하여 DDoS (분산 서비스 거부) 공격 시나리오에 대처할 수 있는 기능도 제공합니다. 

자세한 내용은 DDoS 탄력성에 대한 AWS 모범 사례 백서를 참조하십시오.

Amazon API Gateway를 사용하여 선택적인 메모리 캐시에 응답을 저장하여 특정 API 요청의 성능을 향상시킬 수 있습니다. 

이 방법은 반복 API 요청에 대한 성능 이점을 제공 할뿐만 아니라 Lambda 함수 실행 횟수를 줄여 전체 비용을 줄일 수 있습니다.

<br>
<br>

##### 액세스 제한
Amazon API Gateway는 API 키 생성 및 이러한 키와 구성 가능한 사용 계획의 연관을 지원합니다. 

Amazon CloudWatch로 API 키 사용을 모니터링 할 수 있습니다.

API Gateway는 API의 각 메서드에 대한 제한, 속도 제한 및 버스트 속도 제한을 지원합니다.

Amazon API Gateway와 함께 AWS WAF (Web Application Firewall)를 사용하여 API를 공격으로부터 보호 할 수 있습니다.

###### 서버리스 데이터 스토리지 옵션

- Amazon S3는 업계 최고의 확장성, 데이터 가용성, 보안 및 성능을 제공하는 객체 스토리지 서비스입니다.
  
- Amazon Aurora는 클라우드용으로 구축 된 MySQL 및 PostgreSQL 호환 관계형 데이터베이스로, 기존 엔터프라이즈 데이터베이스의 성능과 가용성을 오픈 소스 데이터베이스의 단순성과 비용 효율성과 결합합니다.      Amazon Aurora는 서버리스 및 기존 사용 모델을 모두 제공합니다.

- Amazon DynamoDB는 모든 규모에서 한 자리 밀리 초 성능을 제공하는 키-값 및 문서 데이터베이스입니다. 
  
내장 된 보안, 백업 및 복원, 인터넷 규모의 애플리케이션을위한 인 메모리 캐싱 기능을 갖춘 완전 관리형 서버리스 멀티 리전 멀티 마스터 내구성 데이터베이스입니다.

- Amazon Timestream은 IoT 및 운영 애플리케이션을 위한 빠르고 확장 가능하며 완벽하게 관리되는 시계열 데이터베이스 서비스로 관계형 데이터베이스 비용의 1/10에 하루에 수조 건의 이벤트를 쉽게 저장하고 분석 할 수 있습니다.

IoT 장치, IT 시스템 및 스마트 산업 기계의 증가에 따라 시간이 지남에 따라 변화하는 방식을 측정하는 데이터 인 시계열 데이터는 가장 빠르게 성장하는 데이터 유형 중 하나입니다.

- Amazon QLDB (Amazon Quantum Ledger Database)는 중앙에서 신뢰할 수 있는 기관이 소유한 투명하고 불변의 암호로 확인할 수 있는 트랜잭션 로그를 제공하는 완전 관리형 원장 데이터베이스입니다.

Amazon QLDB는 모든 애플리케이션 데이터 변경 사항을 추적하고 시간이 지남에 따라 완전하고 검증 가능한 변경 기록을 유지합니다.

##### 비 서버리스 데이터 스토리지 옵션

- Amazon Relational Database Service (Amazon RDS)는 사용 가능한 엔진 (Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle 및 SQL Server)을 사용하여 관계형 데이터베이스를 보다 쉽게 설정, 운영 및 확장 할 수 있는 관리 형 웹 서비스입니다.
  
메모리, 성능 또는 I / O에 최적화 된 여러 다른 데이터베이스 인스턴스 유형에서 실행됩니다.

- Amazon Redshift는 클라우드에서 완벽하게 관리되는 페타 바이트 규모의 데이터웨어 하우스 서비스입니다.

- Amazon ElastiCache는 Redis 또는 Memcached의 완전 관리형 배포입니다. 널리 사용되는 오픈 소스 호환인 메모리 데이터 저장소를 원활하게 배포, 실행 및 확장 할 수 있습니다.

- Amazon Neptune은 빠르고 안정적인 완전 관리형 그래프 데이터베이스 서비스로, 고도로 연결된 데이터 세트와 함께 작동하는 애플리케이션을 쉽게 구축하고 실행할 수 있습니다.

- Amazon Neptune은 널리 사용되는 그래프 모델인 Property Graph 및 W3C의 RDF와 각각의 쿼리 언어를 지원하므로 고도로 연결된 데이터 세트를 효율적으로 탐색하는 쿼리를 쉽게 작성할 수 있습니다.

- Amazon DocumentDB (MongoDB 호환)는 MongoDB 워크로드를 지원하는 빠르고 확장 가능하며 가용성이 높으며 완전히 관리되는 문서 데이터베이스 서비스입니다.
마지막으로 Amazon EC2에서 독립적으로 실행되는 데이터 저장소를 멀티 계층 애플리케이션의 데이터 계층으로 사용할 수도 있습니다.


##### 프리젠테이션 계층

프리젠테이션 티어는 인터넷을 통해 노출 된 API Gateway REST 엔드 포인트를 통해 로직 티어와 상호 작용합니다.

- Amazon Cognito는 서버없는 사용자 자격 증명 및 데이터 동기화 서비스로, 웹 및 모바일 앱에 사용자 가입, 로그인 및 액세스 제어를 빠르고 쉽게 추가 할 수 있습니다. 

Amazon Cognito는 수백만 명의 사용자로 확장하고 Facebook, Google 및 Amazon과 같은 소셜 자격 증명 공급자 및 SAML 2.0을 통한 엔터프라이즈 자격 증명 공급자와의 로그인을 지원합니다.

- Amazon CloudFront가 포함 된 Amazon S3 : Amazon S3를 사용하면 웹 서버를 제공하지 않고도 S3 버킷에서 직접 단일 페이지 애플리케이션 (SPA)과 같은 정적 웹 사이트를 제공 할 수 있습니다. 

- Amazon CloudFront를 관리형 콘텐츠 전송 네트워크 (CDN)로 사용하여 성능을 향상시키고 관리 형 또는 사용자 지정 인증서를 사용하여 SSL / TL을 활성화 할 수 있습니다.


네트워킹 구성 및 애플리케이션 요구 사항에 따라 Amazon API Gateway API가 CORS (Cross-Origin Resource Sharing)와 호환되도록 해야 할 수 있습니다. 

CORS 준수는 웹 브라우저가 정적 웹 페이지 내에서 API를 직접 호출 할 수 있도록 합니다.

Amazon CloudFront가있는 웹 사이트를 배포하면 애플리케이션에 도달 할 수 있는 CloudFront 도메인 이름 (예 : d2d47p2vcczkh2.cloudfront.net)이 제공됩니다. 

Amazon Route 53을 사용하여 도메인 이름을 등록하여 CloudFront 배포로 보내거나 이미 소유 한 도메인 이름을 CloudFront 배포로 보낼 수 있습니다. 

이를 통해 사용자는 익숙한 도메인 이름을 사용하여 사이트에 액세스 할 수 있습니다.

Route 53을 사용하여 사용자 지정 도메인 이름을 API Gateway 배포에 할당하면 사용자가 익숙한 도메인 이름을 사용하여 API를 호출 할 수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/156179992-88eb3141-7137-4600-b090-47eb11ee330d.png)

![image](https://user-images.githubusercontent.com/62640332/156180037-4ab65153-a180-41a9-bb38-3cac7ff334d5.png)

![image](https://user-images.githubusercontent.com/62640332/156180074-b3fc30a1-474b-46d0-970c-85888ae59044.png)

![image](https://user-images.githubusercontent.com/62640332/156180131-675e2ec8-1ac9-49f4-931d-f592499d9453.png)

![image](https://user-images.githubusercontent.com/62640332/156180185-327efc2d-310c-4f59-acdb-27c360a4d7fe.png)

