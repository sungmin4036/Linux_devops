![image](https://user-images.githubusercontent.com/62640332/155704961-858b2b1e-2a92-4909-b559-ba8f1d0648b2.png)


사용자가 들어오면, AG(가용성 존)에의해서 나눠진다. 이 가용성 존은 독립적인 데이터센터 또는 서버클러스터 구성하고 있습니다.

=> 고가용성 확보, 논리적으로 하나의 네트워크처럼 보이나, 실제로는 여러 분산되어있는 서버자원을 통해 고가용성 확보 가능합니다.

Edge Location도 분산되있기 떄문에 많은 유저들이 빠르게 컨텐츠 Access 가능합니다.

  
![image](https://user-images.githubusercontent.com/62640332/155705417-b6211b5b-98fd-4f4a-b07a-79811b7cbc07.png)

![image](https://user-images.githubusercontent.com/62640332/155705572-b21cef6c-892a-46b0-bc0f-04b3dc465cbf.png)

![image](https://user-images.githubusercontent.com/62640332/155705767-db5644dc-c934-4ce3-a7ab-1b55aa60b1ae.png)


![image](https://user-images.githubusercontent.com/62640332/155706137-34e2bfc0-0b89-49f3-8971-8d49a3ecc059.png)

![image](https://user-images.githubusercontent.com/62640332/155706262-107fc75a-7f5e-4119-836c-c933ea34d2d6.png)

![image](https://user-images.githubusercontent.com/62640332/155706285-1372b0c0-a852-4167-804d-1ef0d216c90a.png)

![image](https://user-images.githubusercontent.com/62640332/155706322-df5d06e4-649d-4781-ab8b-d89397979fc1.png)

![image](https://user-images.githubusercontent.com/62640332/155706402-ecfd3bee-62a0-4df8-b65b-b82a5d4df036.png)

대부분 RDB로 해결 가능합니다.  read많고  write 이 적은 서비스의 경우 RDB로 충분

데이터가 갑자기 늘어난다거나, 유저의 write가 해비한 경우 nosql 고려 => dynamoDB

![image](https://user-images.githubusercontent.com/62640332/155706628-a9a37545-9107-4d78-8de9-afcdb197fb1a.png)

dynamo db의 장점은 read capacity와 write capacity를 다이얼 돌리듯이 언제든지 변경 가능

![image](https://user-images.githubusercontent.com/62640332/155706811-ddb9ec91-4c82-495d-bbd7-467e53f441f2.png)

기존의 많은 게임회사는 mssql 사용하는데, db 사이즈가 커지면 db Latency가 커질수 바께 없다.

 
![image](https://user-images.githubusercontent.com/62640332/155706919-f7b9657a-aa64-4444-b147-cbcade5c5d36.png)

그러나 dynamo DB는 db사이즈가 커져도 db Latency가 10ms 언더로 제공되는 장점이 있다.


ㅁ 사용자 100 이상

![image](https://user-images.githubusercontent.com/62640332/155707030-ca51c974-ad05-4aac-be28-ff74abce2d4b.png) 

사용자가 많아짐에따라  가급적이면 애플리케이션에 집중하고, 인프라 관리는 aws 맡긴다.

![image](https://user-images.githubusercontent.com/62640332/155707091-18363221-faf8-4e9d-bd9b-52e31856710a.png)

<br>
<br>
<br>

ㅁ 사용자 1000 이상

![image](https://user-images.githubusercontent.com/62640332/155707152-176c1445-0fea-4f03-b662-f78c95e7c1b6.png)

서버를 나누고 로드 밸런서를 도입하여 고가용성을 얻는다.

두개 이상의 인스턴스에 로드밸런싱이 가능하며, 로드밸런스의 장점은 오토스켈링이 가능합니다.

하나는 A존과 B존에 있는데, 이것은 독립된 데이터센터에  두개의 서버가 따로 분리되있고 그것을 조절이 가능하다.

즉 존하나가 없어지더라도 고가용성 확보가 가능합니다.

AWS 기본적으로 로드밸런싱과 멀티 에이지를 사용하여 고가용성을 확보할수있다.

그리고, DB는 이중화하는 것이 좋습니다.

<br>
<br>
<br>

위 방법으로는 유저 10,000 명 까지는 버틸수 있으나 

![image](https://user-images.githubusercontent.com/62640332/155707522-069d303f-2dd2-4cd4-84b5-ab6ad71ccde4.png)

인스턴스와 db를 늘리기 시작하면 됩니다.

![image](https://user-images.githubusercontent.com/62640332/155707566-032addad-ff5b-4128-94e6-4d8de7f0f7eb.png)

수평 수직 확장을 만족하는 인프라가 완성됩니다.

사실 이 정도는 DB 작업을 쉽게 가능하게 했을뿐 크게 다르지 않습니다.

이젠 높은 성능과 가용성을 위한 고급 클라우드 아키텍처 필요.

![image](https://user-images.githubusercontent.com/62640332/155707696-2ec5b103-b493-428c-b52a-c039f6e8304e.png)

로드밸런스 앞,뒤에 있는 로드를 줄여야 합니다. 처음시작시 대부분 인스턴스에 웹서버, DB, 파일서버로도 운영하는등 가볍지 않은것들이 매우 많이 있습니다.

애플리케이션 하나만 있어야하는데 많이 설치되어있다 ==> 인스턴스를 가볍게 하여 비용절감과 손쉽게 확장, 성능 최대로 할수 있게 해야합니다.

![image](https://user-images.githubusercontent.com/62640332/155707875-f491e4f5-60d7-4c74-bc79-ca2120896f4f.png)

가장먼저 해야할일은 인스턴스에 설치되있는 유저데이트, 업로드된 파일, 볼륨에 들어있는 파일을 S3에 해당하는 스토리지 서비스에 옮겨야합니다.

이렇게 하면 가벼운 애플리케이션만 가동되고, 나중에 오토스켈링 적용시 쉽게 가능해진다.

그리고 CloudFront 서비스 통해서 유저 이미지, 스태틱 파일들을 접근할수 있게 할수있습니다.

 
![image](https://user-images.githubusercontent.com/62640332/155708158-ddf80e41-63bb-400b-bf0c-18619c6302a1.png)

DB 분산 하기위해 캐시를추가 합니다. "ElasticCahe" 를사용하거나 DynamoDB 이용

 
![image](https://user-images.githubusercontent.com/62640332/155708233-e4475998-62c1-4586-bc74-a6305a1a6711.png)


![image](https://user-images.githubusercontent.com/62640332/155708385-e0666e15-0fb7-40f3-ae89-eef77becb737.png)


네트워크 트래픽, CPU 사용량등에 따라서 스켈링을 할수 있도록 정책을 정하면 사용 가능합니다.

![image](https://user-images.githubusercontent.com/62640332/155708521-026c557d-8a3a-4b47-9d82-e3fe0ae4a053.png)


---

![image](https://user-images.githubusercontent.com/62640332/155710579-ecdb29c3-4ae3-422f-8298-f9de3e40e5a3.png)

ㅁ 비지니스에만 집중하기 위한 클라우드 네이티크 전략

![image](https://user-images.githubusercontent.com/62640332/155710789-2e41dae1-7544-4dca-9bf3-6e9312286a15.png)

![image](https://user-images.githubusercontent.com/62640332/155710834-673c139b-35fe-4d19-98e2-2f4b9a0182ee.png)

개발에만 집중하기위해, 자동화된 개발, 배포, 구축 자동화가 필요.

 
![image](https://user-images.githubusercontent.com/62640332/155710947-2ea8fc29-7e32-41e6-8352-71e314ca3c4c.png)


![image](https://user-images.githubusercontent.com/62640332/155711103-f00d66bb-bfd2-4923-9511-803c06acd167.png)

![image](https://user-images.githubusercontent.com/62640332/155711290-80e140d8-7bdf-4bb0-9642-501fd226fb07.png)


![image](https://user-images.githubusercontent.com/62640332/155711344-6c6673da-3487-4b12-b081-c71708e183ad.png)

![image](https://user-images.githubusercontent.com/62640332/155711366-81a5a9bd-0ed6-4e59-9020-fa5e4edc7271.png)

실제 600만 사용자 클라우드 아키택처 입니다.

![image](https://user-images.githubusercontent.com/62640332/155711490-022b59ac-3f4b-4fc6-8835-8ec49a59d58b.png)

서버에 컨테이너를 배포하여, 서버의 효율성을 높인다.  EC2 container service 를 활용하여, docker 컨테이너들을 빠르게 배포하여

손쉽게 애플리케이션들을 가상서버에 배포하고, 서버의 인스턴스 효율 높인다.

![image](https://user-images.githubusercontent.com/62640332/155711651-f72ede60-fa4e-4688-af8c-698c5f27a3cf.png)

람다는 코드를 업로하면 코드를 실행만 하는것입니다.  서버, 확장성, 코드 다필요없습니다.

유저가 원하는 기능을 동작만하고 끝나고, 그리고 과금시에도 람다 코드가 실행되는 시간단위 과금합니다. => 효율적으로 가능

 
![image](https://user-images.githubusercontent.com/62640332/155711880-25407244-9938-47de-975c-32b7beafbe57.png)


기존에는 서버 하나 추가하는데 몇주가 걸렸지만, EC2만으로 몇분만에 가능해지며, Ec2 컨테이너 서비스는 몇초만에 가능해집니다.


![image](https://user-images.githubusercontent.com/62640332/155711916-7c26d3aa-ea7a-45c8-8cb4-f334e85b6e3a.png)

![image](https://user-images.githubusercontent.com/62640332/155711959-c8b7a393-b6df-47dc-a76f-9018e02e6fb4.png)

![image](https://user-images.githubusercontent.com/62640332/155711989-5b7200d0-0356-4283-b9b7-a1d849e28cba.png)

앞단에 API Gateway만 있고 뒤에 람다만통해서 서버없이 마이크로 서비스 운영이 됩니다.

 
![image](https://user-images.githubusercontent.com/62640332/155712050-157bcc38-0268-418e-802c-3bf5a5f0ec9d.png)


웹서비스 만들었을때는 웹사이트는 S3에 있고, static web hosting을 통해서 html,js,css는 S3에 두고 API gateway, lamda, dynamoDB 등을 통해서 뒷단을 구성하게 됩니다.

그러면 서버 구성없이도, 손쉽게 웹서비스 마이크로 서비스로 운영 가능해진다.

![image](https://user-images.githubusercontent.com/62640332/155712228-8064e61a-53c7-467e-9183-337428ca1a71.png)

모바일도 API Gateway, lamda를 통해서 만들기 가능하며, 유저는 아마존 코버네트 같은 인증을 통해 손쉽게 access 가능해진다.

![image](https://user-images.githubusercontent.com/62640332/155712396-a9949c99-c432-46a0-8647-51ab4cc4cbe0.png)

![image](https://user-images.githubusercontent.com/62640332/155712448-7dd28d39-b5d4-4a45-bfa9-7c31d211b4cd.png)