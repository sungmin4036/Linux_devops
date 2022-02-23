- CI/CD : 개발자가 코드 변경사항을 중앙 repository에 정기적으로 벙합하고, 빌드와 테스트 과정을 거쳐 프러덕션에 릴리스하는 각 과저을 포함한다.
- IaC: 버전 제어, 지속적 통합과 같은 코드 및 소프트웨어 개발 기법을 사용하여 인프라를 프로비저닝하고 관리

![image](https://user-images.githubusercontent.com/62640332/155364530-06abcc20-8a38-4274-a025-9ca8028b445a.png)



애플리케이션을 관리한다는 것은 로직을 구성하는 코드 뿐만아니라 해당 애플리케이션 인프라 관리도 포함한다.

이는 AWS위에 웹 애플리케이션을 구축했다고 가정한다면, VPC, EC2, RDS등도 관리해야한다는것이 된다.

![image](https://user-images.githubusercontent.com/62640332/155365137-ee2bad50-f1c1-4b8c-8c8b-769f9cd0b93f.png)

초보자는 GUI 방식이 좋지만, 자동화 하기위해서는 CLI 방식으로 해야한다.

 CLI 명령어 똑같이 재현 어려움, SDK 자원의 자동생성을 지원하고 벌크형태로 만드는게 가능하지만, aws에 대한 엑세스 용도로만 사용되는경우가 대부분

 그래서 AWS  CloudFomration 이나 AWS Cloud Development Kit(AWS CDK)로 만드는게 devops의 모범사례에 적합하다.

 


 대부분 devops팀이 겪는 3R problem
 
 - repeatability(반복성)
 - reproducibility(재현성)
 - reliability(신뢰성)

사람들이 같은 반복적으로 같은 환경 만들시 휴먼에러로 가끔 다른 결과 나오는 경우가 있다. 또 개발환경에서는 문제가 없었지만 프러덕션 환경에서는 문제 발생하는 경우가 있다.

이러한 인프라 관리에서 발생할수 있는 문제를 IaC 관리를 통해 아예 제외시킬수 있다.

IaC는 소스코드처럼 버전닝 및 관리가 되어, 환경내에 일어나는 일 추적이 가능하다. 즉 변경기록과 롤백이 비교적 쉽다.

또한 동일한 config 파일, 코드만 있다면 여러개의 환경을 만들어 낼수 있다.

AWS 에서 IaC 방식 : AWS CloudFormation, AWS CDK
Third-pary tool : HashiCorp Terraform, puppet

![image](https://user-images.githubusercontent.com/62640332/155366962-ed6c89dd-d684-4b10-8bbc-db8c28e132a0.png)

앱 코드, 인프라 템플릿 작성후 배포하고, 실제환경에서 사용하고 다시 최적화를 위해 반복


![image](https://user-images.githubusercontent.com/62640332/155367166-a0532aeb-e247-47bb-89f2-2df1b6eff307.png)

AWS resource를 모델링하고 설정하여 리소스 관리에 대한 신경을 경감시키고, 해당 리소스위에 애플리케이션에 집중할수 있게하는 서비스

아마존 EC2, RDS 관련값을 기재한 템플릿을 생성하면 클라우드 포메이션이 해당 리소스에 프로비저닝과 구성을 담당한다.

클라우드 포메이션을 사용하면 aws 리소스를 사용자가 개별적으로 생성하고 구성할 필요가 없으며, 관련된 dependency에 대해 파악할 필요가 없어진다.

![image](https://user-images.githubusercontent.com/62640332/155367900-879e7a51-7d96-4b27-935b-dc795ba6406a.png)

![image](https://user-images.githubusercontent.com/62640332/155367949-05e92a3a-8b5f-4db8-bc21-e799b2c8332e.png)

코드는 템플릿이라는 파일에 기록이 되고, 스택은 탬플릿을 통해 발생된다.

탬플릿은 주로 애플리케이션을 위한 리소스를 정의합니다. 리소스의 상태를 설명

각 리소스는 사용가능한 속성에 따라 구성되고, 종속석을 명시적으로 선언하거나 암묵적으로 검색 가능하다.

클라우드 포메이션은 이러한 의도를 API 호출로 변환하여 리소스를 프로비저닝 한다.

위의 예시는 Yalm 

- 버전은 선택사항 
- 설명은 상세히 적는것 중요
- 파라미터는 해당 탬플릿 사용하는 사용자에게 하는 질문
- 매핑은 스스로 채워지는 파라미터와 같다. ex) 매핑마다 리전마다 구동할 AIM을 명시해 놓으면 서울 리전 안에서 새로운 탬플릿 만들때 실행할 AIM을 자동적으로 선택된다.
- 조건의경우 참인경우에만 리소스 생성
- 리소스는 탬플릿에서 유일하게 필수로 작성해야하고, 생성할 aws 리소르를 기재하는 부분
- 아웃풋은 리소스가 프로비저닝된 후에도 도출될수 있는 값

![image](https://user-images.githubusercontent.com/62640332/155369312-a21a1a43-31da-4332-a2c0-09b4c5df9e15.png)

![image](https://user-images.githubusercontent.com/62640332/155369769-f6dc5735-d9da-4447-9648-7cab30c8b1e5.png)

![image](https://user-images.githubusercontent.com/62640332/155369871-0d763734-b57b-4792-be21-ae3aa4fbdb07.png)

![image](https://user-images.githubusercontent.com/62640332/155369990-f010aad0-9fd8-4391-86a7-2e225a1297ad.png)

이미 aws 해비 사용자라면 , 여러개의 개정을 가지고, 하나의 개정에서 여러개의 리전 사용할것이다. 이떄 aws cloud formation을 어떻게 확장해서 사용할수 있을까?

=> Stack set

statck set 들을 위한 stack을 생성 가능하다. 스택셋 만든후에는 언제든지 추가 계정 및 리전에 대한 스택을 추가 가능하다.



탬플릿을 생성후 바로 클라우드 포메이션에 업로드는 가능하지만, 해당  탬플릿이 에러가 발생한다면 리소스가 프로비저닝이 불가능해진다.

로직 단에서의 에러는 탬플릿 작성전에 완료 되어야한다. ==> 오픈소스 툴인 cfn-lint 

해당 툴 사용하면 탬플릿 배포전에 에러를 인지하고 수정이 가능해진다.

![image](https://user-images.githubusercontent.com/62640332/155370837-e0326487-1ab7-4e2a-b791-5768b721e7b8.png)


처음 소규모의 리소스 프로비저닝에는 하나의 플랫폼에 관련 정보를 모두 명시하는것이 효율적이지만, 워크로드의 범위가 커지면

계층화된 아키텍처 방식으로 만들어야 한다. 

생김새가 비슷하여 cloud formation cake 라고도 불린다. 성격이 비슷하고, 리소스의 라이프 사이클 비슷한것 끼리 묶어서 사용해야 한다.

![image](https://user-images.githubusercontent.com/62640332/155371098-19e7e379-84ed-4423-a59a-8e7593abb4ed.png)

예를 들어 네트워크 보안시 애플리케이션 바이너리 파일 및 데이터베이스 변경시 함께 변경될 가능성 적음

이렇게 각각 분리하면 실수로 자원 삭제 막을수 있고, 필요한 부분만 업데이트 가능하여 이후 리소스 테스트 및 트러블 슈팅 용이 해진다.

IaC 이상적인 가정은 리소스 관련 작업은 IaC를 통해서 하는것이다. 그러나 급하게 일부 변경사항 있을경우 수동으로 리소스 컨트롤 한다.

이와같이 관리되지 않는 변경은 실행중인 스택의 구성이 템플릿에서 드리프트 되었기 때문에 기존에 의도한 상황과 실제 리소스 상태가 불일치 하게 된다.

==> 드리프트 감지기 사용 한다. 

![image](https://user-images.githubusercontent.com/62640332/155371624-bd31a8bd-50e1-46d1-ab69-6cd7a424e94b.png)


db의 패스워드와 같이 중요정보는 하드코딩하지 않는다. 이와같이 빨간글씨 처럼 aws 시스템 매니저 파라미터 스토어에 저장하고 불러오는것 중요

![image](https://user-images.githubusercontent.com/62640332/155371822-f02b0e2b-0c5f-41c2-b2f8-43cbf2ae343c.png)

<br>
<br>
<br>

![image](https://user-images.githubusercontent.com/62640332/155372016-4f2fb894-7283-48ee-8496-199c7fb340d8.png)

![image](https://user-images.githubusercontent.com/62640332/155372086-5a23c594-4a74-438b-aa71-982b7a6976b5.png)

- core framework를 이용하여 하나이상의 stack 포함된 앱을 만들고 구성 가능

stack은 여러 리소스를 포함하고, one to one 클라우드 포메이션 스택과 맵핑된 단위를 의미

- AWS Construct Library는 aws에서 특정 서비스에 대한 리소스를 생성하기 위해 만들어진 구성요소의 집합으로, 프로젝트에 필요한 dependency만 사용할수 있다.

또한, 사용편의성 및 빠른 반복주기에 적합하도록 모범 사례를 기반으로 만들어져 있다.

- AWS CDK CLI는 core framework와 상호작용하며, 프로젝트에 스트럭쳐를 초기화하고 배포된 상태와 배포할 상태를 비교하여, aws 환경에 쉽게 리소스 반영할수 있도록 지원합니다.

![image](https://user-images.githubusercontent.com/62640332/155372817-96c2c14b-518b-4166-ba55-a842eca4fce5.png)


