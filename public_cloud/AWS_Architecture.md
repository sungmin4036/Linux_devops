![image](https://user-images.githubusercontent.com/62640332/155164899-399b00cc-656c-41f4-8395-3d7e26d90e20.png)

기존 개발 방식은  Monolith  앱 방식으로 하나의 소스 코드 배포 단계를 모든 개발자가 달라 붙어서 하게된다. => 속도 감소, 어려움

![image](https://user-images.githubusercontent.com/62640332/155165099-4a58c97a-d6ab-4050-a0aa-7c8248869541.png)

마이크로 서비스 대표적인 예로 NETFLIX, Amazon, Nike...

국내 AWS 고객사: 우아한 형제들, SBS...

- 마이크로 서비스란?
1. service-oriented architecture(서비스들이 네트워크를 통해 서로 통신한다.)
2. composed of
3. loosely coupled elements(서비스는 독자적으로 업데이트하며 서로 영향을 주지 않는다.)
4. that have
5. bounded contexts(다른 서비스의 내부 구조를 알지 못해도, 내 서비스 코드를 업데이트 할수 있다.)

![image](https://user-images.githubusercontent.com/62640332/155165641-2e5909fd-8e42-44b2-8b08-affd176253f0.png)

API로 처리한다.

![image](https://user-images.githubusercontent.com/62640332/155165704-c8dac2bd-5256-4c35-8984-436e7f4b7071.png)

서로간의 인터페이스를 가지고 연결되어 사용된다.

![image](https://user-images.githubusercontent.com/62640332/155165808-6c6fc58a-cb78-45b5-ad97-ad6fede5f7c1.png)

사각형이 전부 각 다른 팀이 서비스 개발한다.

![image](https://user-images.githubusercontent.com/62640332/155165856-d296d69b-1726-4c94-aa28-e494da694032.png)

![image](https://user-images.githubusercontent.com/62640332/155166069-f0c703d0-1f5f-42d7-95aa-a6215dace5d9.png)

EC2로 하고, 오토 스켈링 걸어두고, 앞단에 로드 밸런싱 해논다.

![image](https://user-images.githubusercontent.com/62640332/155166193-fd4c0e71-421a-400b-ad98-6b6ae5b63536.png)

![image](https://user-images.githubusercontent.com/62640332/155166288-6f5ee105-0427-448b-9013-5b432e14f7a8.png)

마이크로 서비스를 하기위해서는 반드시 API를 사용해야하고, 유지비용을 낮추기 위해서  API Gateway 필요

![image](https://user-images.githubusercontent.com/62640332/155166393-8f17eec1-a90a-4e51-a441-b3c92bc70ecf.png)

추가로 컨테이너로 배포로 하는게 더 저렴하게 이용가능하다.

![image](https://user-images.githubusercontent.com/62640332/155166570-0b2ae8e9-c50a-46bb-acba-0984d325250e.png)

serverless 는 DB를 aws 에서 관리해주고, 코드만 관리해주면 되는 방식, 함수단위로 운영

![image](https://user-images.githubusercontent.com/62640332/155166824-c429220c-7a82-4f22-a9cc-d4192a774e31.png)

lambda blueprints를 이용해 써드파티의 slack 과 같은걸 연계해서 사용할수도 있다.

<br>
<br>
<br>
<br>
<br>

![image](https://user-images.githubusercontent.com/62640332/155167223-5a30eedd-d651-4d6f-8b4b-fad13c7b670d.png)

기존 빙글회사 모놀리식 

모든 백엔드, 프론트앤드 아키텍처는  전부 로드 밸런스로 들어가고, 로드밸런스가 트래픽을 분산시키며 EC2로 이동시키고, EC2는 aws opsworks에 의해 디플로이, 배포를 해줬다.

몇몇 회사는  Elastic beanstalk 사용하지만, 커스터마이징이 많으면 Opsworks 사용, 그뒤에 다양한 DB와 캐시를 각각 EC2 인스턴스들이 접근

![image](https://user-images.githubusercontent.com/62640332/155168181-0e5084c9-d1e0-4f4f-b150-e3b0dba204c7.png)


repository 하나에 모든 기능이 다 들어가 있었다.

정말 조금만 수정을해도 최소 1시간 이 넘게 테스트를 했어야 했다.

ㅁ 기존 개발 환경의 문제점

1. 언제나 모든 서비스를 함께 Deploy / Scale ---> 상황에따라 그떄그때 요구되는양이 달라질수 있는데, 이렇게 한번에 하면 스켈링을 한번에 해야한다.
2. 새로운 기술 도입이 어려움 --- 파이썬은 머신러닝 라이브러리가 많고,  비동기처리는 node.js, 새로운 프론트 앤드도입등 매우 힘든 상황

==> 개발자들의 기술적으로 자유로운 사고가 어려워서 생산성을 저하시킨다.

![image](https://user-images.githubusercontent.com/62640332/155169155-427106b1-5256-460c-89a5-2f74f0534440.png)

프론트 앤드 개발자가 루비코드를 봐야하고, 피드 알고리즘 전문가가 다른거 까지 봐야하는 문제가 발생

프론트 앤드만 사용하는 cpu 사용량등 보기 불가능

![image](https://user-images.githubusercontent.com/62640332/155169202-9bdaf3cd-bb6b-490c-ae2e-dbeb48753512.png)

마이크로 서비스가 좋지만, 구축하기가 매우 어려움

서비스가 장기적으로 몇백개가 되는데 모든걸 스켈링, 디플로이 등을 관리해줘야한다. ==> 자유로운 배포와 스켈링이 가능한 탬플릿을 만들고, 마이크로 서비스를 찍어내자

API GATEway에 람다 붙이고, 람다에 엘라스틱 같은걸 붙여서, 서버리스 프레임워크를 사용.

서버리스 프레임워크는 클라우드 포메이션을 지원해서 API게이트웨이에 추가적으로 RDS에 대한 정의를 JSON파일로 지정하고, 그걸 배포하면 자동으로  리소스 만들어 주게 만듬

=> Infrastructure를 code로 관리가 가능하게되었다. 기존 RDS 수동으로 할때 손이 많이가고, 개발서버 따로 있을경우에는 프러덕션 서버가 세팅이 계속 달라진다거나 하는 문제 해결하여

어떤 변경, 언제 변경을 한눈에 트래킹 가능하게 되었다. 

또 하나는 Dynamo DB를 사용하는경우에는 자동으로 스케링 되어 건드릴 필요가 없게 된다. 

배포도 한번 올리면, DB 인스턴스 껏다가 키던가, 도커이미지 바꾸거나 해야되지만, 람다는 코드 업로드만하면 무중단으로 배포된다.

![image](https://user-images.githubusercontent.com/62640332/155170635-b9513a35-835d-4531-9b3f-d8af8f7b3c82.png)

모두가 사용하는 앱 특성상 스팸이 새벽시간대에 많이 올라온다. => 스팸 필터를 별도의 마이크로 서비스로 만든다.

dynamoDB에다가 스팸으로 지정한 URL 을 목록 등록후, API에서 어떤 텍스트 오면, 람다에서 텍스트 분석해서 URL 뽑아내고, DB에 매치되는거 있을시 스팸 인지


이 마이크로 서비스는 개발을할때 진가가 발휘 되었다.

![image](https://user-images.githubusercontent.com/62640332/155171381-384f6bc9-dfea-405d-bc07-5dd8e1d60109.png)

스팸이 대부분 short를 사용하였고,  인프라 스트척쳐, 배포 같은거 신경안쓰고 로직에만 집중할수 있게 되었다.

![image](https://user-images.githubusercontent.com/62640332/155171561-70d041e5-b0f5-4092-b822-b4cdf3bedd50.png)

spamChecker를 사용하면서 상태를 따로 확인을 원했는데 가능하게 됨.

![image](https://user-images.githubusercontent.com/62640332/155171653-5428421e-bb30-4698-8973-ec59860835a1.png)

도메인 단위로 나눠서 하기 시작

![image](https://user-images.githubusercontent.com/62640332/155171735-dd49d694-3930-42b1-8558-65643bf619ab.png)

strangler pattern Architecture란? 모든 서비스에대한 요청을 하나의 API 서버로 들어오고 이 API 서버가 필요로 하는 서비스들로 처리된다.

 만약 feed를 요청했다면, feed 요청이 api서버로 가고, api서버에서 feed서비스 api 달라고 요청

- cloudwatch 사용하여 문제 생기면 알람을 slack으로 바로오게 설정

- dependency문제 - 어떤 서비스가 죽었을때 어떤서비스가 영향을 줄까를 파악하기 매우 힘듬 그래서 다이어 그램을 만들어서 한눈에 볼수있게함.

Lesson Learned - Microservice Migration

1. 새로 추가되거나 변경이 필요한 기능부터 하나씩 분리
   - 잘 돌아가고 있는 오래된 기능을 재작성은 리스크 높고, 이득도 적음---> 개발자로서의 목적은 유저에게 가치를 제공하는거지 새로운 기술 사용하는것 X, 새로만들거나 장기적으로 변경이 유리할꺼같다고 생각한거 우선으로 만들기 시작
   - 리스크가 작은 것부터 시작해서 팀 단위 운영/개발 노하우 쌓아야 한다.

2. Business Domain 단위로 서비스 구성 --> 서비스를 어떻게 쪼개야 할지 정하기 힘듬
   - Cache (x), DB(x) --> 만약 캐시나 DB로 했다면, 여러 서비스가 캐시를 이용한다면 캐시 문제생길시 다 영향을 간다.
   - Feed (O), Notification(O) --> 독립적으로 개발, scale, 배포 되는 단위

3. 서비스의 존재 목적은 사용 되어지는 것  
   - 다른 개발자 / 다른 시스템에서 쓰기 쉽도록 인터페이스 갖춰야한다.
   - REST API라면 Swagger등을 통해 적극적으로 문서화


AWS에서 제공시 좋은것

1. AWS에서 제공하는 Fully-managed service 적극 활용
   - Search Microservice -> Lambda + cloudsearch
   - Feed Microservice -> Lambda + DynamoDB + Kinesis Stream

2. infrastructure-as-a-Code (AWS CloudFormation)
   - 시간이 조금만 지나면 AWS내 많은 리소스 관리 어려워짐
   - AWS CloudFormation을 통해 리소스들을 코드화/ 데이터화

