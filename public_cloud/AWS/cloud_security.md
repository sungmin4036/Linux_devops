- [전체 보안 흐름](#전체-보안-흐름)
- [Amazon GuardDuty 란?](#amazon-guardduty-란)

---


![image](https://user-images.githubusercontent.com/62640332/156534350-87be6209-cdb8-4651-a037-42b0f1a6561f.png)


### 전체 보안 흐름

amazon GuardDuty 에 대해 이야기하기 전에, 우선 AWS 에서 가이드하는 시큐리티 대응 전체과정에서 Amazon GuardDuty 가 어떤 역할을 담당하고 있는지를 알아둘 필요가 있습니다.

위 그림을 보시면, 시큐리티 대응 과정은 크게 5가지 단계로 분류됨을 알 수 있습니다.

1. Identify (식별)
   
시큐리티의 대상이 될 리소스를 식별하여 관리하는 단계입니다. 여기서 리소스라함은 EC2 나 RDS 등의 인스턴스를 하나의 예로 들 수 있습니다.

AWS 에서는 SSM 과 Config 를 활용하여 리소스를 식별하고 관리할 수 있도록 돕고 있습니다.

2. Protect (방어)
   
식별되어 관리되는 리소스들 중에 방어해야할 리소스들을 선별하여 모든 레이어에서 보호합니다. 

네트워크(VPC) 상에서 전송되는 데이터를 안전하게 보호하거나 접속경로를 차단하는 등의 설정, AWS Shield 를 통한 DDoS 공격 방어, WAF 를 통한 웹방화벽, KMS 를 통한 데이터 암호화 키 보호, IAM 을 통한 권한 관리 등 굉장히 많은 AWS 서비스들이 방어 과정에서 활용될 수 있습니다.

3. Detect (검출)

이렇게 열심히 방어를 했다고 하더라도 간혹 시큐리티 설정 누락 혹은 관리 상의 허점 등으로 인해 시큐리티 사고가 발생할 수 있습니다. 

이런 경우에는 검출되었다는 사실 자체를 깨닫기가 상당히 어려운데요, 이걸 좀 더 쉽고 효과적으로 검출할 수 있도록 도와주기 위해 도와주는 서비스 들 중에 하나가 바로 Amazon GuardDuty 입니다.

4. Respond (반응)

Amazon GuardDuty 가 어떤 역할을 하는지를 알게되었지만, 이왕 설명하는김에 5가지 단계 전부다 한번씩 살펴보도록 하겠습니다. 

시큐리티 사고가 발생했다는 사실을 알게되었다고 하더라도(검출 성공), 그 근본 원인을 조사하는 등의 "Respond(반응)" 과정이 진행되는 것이 일반적입니다.

루트원인을 규명하거나 자동화된 프로세스로 원래 상태로 돌려놓거나 혹은 둘다 동시에 진행하는 등의 과정을 생각할 수 있는데요, 흔히 온프렘 데이터센터 등에서는 시큐리티 사고가 발생하면 우선 랜선을 뽑아버리고 정밀조사를 진행하는 것에 비유될 수 있을 것 같습니다. 

하지만, 클라우드에서는 그런 조치를 취할 수가 없으므로 다른 방식으로 접근해야할텐데요, 대표적인 예로 EC2 침해사실을 알게되었다면 즉시 Security Group 을 다 막아버리는 것으로 랜선을 차단하는 것과 동일한 효과를 누릴 수 있습니다.

그리고 스냅샷을 뜨거나 해서 백도어로 해당 인스턴스를 조사하는 등의 방법이 있을 수 있을 것 같습니다. 

하지만 루트 원인을 규명하는 것은 역시 상당한 수고로움과 난이도를 요구하는 작업입니다. 

AWS 에서는 이러한 시큐리티 사고의 루트 원인을 좀 더 쉽게 규명할 수 있도록 2019년 re:Invent 에서 Amazon Detective 라는 서비스를 새롭게 출시하기도 했습니다. Amazon Detective 에 대해서는 다른 글에서 좀 더 자세히 소개해볼까 합니다.

5. Recover (복구)
시큐리티 사고의 조사과정이 끝났다면 원상태로 복구시킴과 동시에 추가적인 시큐리티 설정을 하게됩니다.

이를 위해 평소에 백업을 잘 해두어야하는데요, 백업을 위한 서비스들로는 각종 서비스들의 Snapshot 이나 Glacier 가 있습니다.

### Amazon GuardDuty 란?

Amazon GuardDuty 란, AWS 계정과 워크로드를 보호하기 위해 악의적 활동 또는 무단 동작을 지속적으로 모니터링하는 위협 탐지 서비스입니다.


![image](https://user-images.githubusercontent.com/62640332/156537077-311bd4a0-e4b9-46da-af90-953cd490af06.png)

GuardDuty 를 활성화시키고 나서 볼 수 있는 메인 화면입니다. 

뭔가 리스트로 잔뜩 표시되어 있는것들은, GuardDuty 가 CloudTrail , VPC Flow Logs, DNS 로그 를 직접 "분석" 해서 위협요소를 탐지한 결과를 나타냅니다. 말이 어렵나요?

그러니까 즉 위의 3가지 로그 데이터를 기반으로 머신러닝 알고리즘을 돌려서 위협이 될만한 요소들을 탐지하고 그것을 리스트로 표현해준다는 것입니다. 

즉, 모든 시큐리티 사고에 대한 탐지를 해주기보다는 AWS 상에서 실행되는 모든 동작(API) 로그(CloudTrail)와, VPC 상에서 일어난 모든 트래픽의 메타데이터(VPC Flow Logs)와, DNS 로그를 기반으로 발견할 수 있는 위협요소만을 대상으로 합니다.

참고로, GuardDuty 에서 사용하는 위의 3가지 데이터소스를 직접 활성화 시키지 않아도 분석이 가능합니다! 

즉, GuardDuty 를 사용하기 위해 Flow Logs 를 활성화시킨다거나, CloudTrail 을 활성화시킬 필요는 없다는 뜻입니다. (물론, GuardDuty 가 "분석" 에 사용하는데에는 지장이 없지만, 추후 추가적인 조사를 위해 따로 활성화시켜두는것이 좋습니다)

하지만, 그렇다고 하더라도 CloudTrail 에서 모든 AWS 상에서의 Management Events 를 분석해주고 Flow Logs 를 통해 VPC 의 모든 트래픽을 분석해주니 기본적인 시큐리티 사고는 거의 다 탐지해준다고 보시면 됩니다! 참고로, S3 의 데이터 중에 개인정보 등의 민감한 정보가 포함되어있는지 자동으로 분석해주는 Amazon Macie 라는 서비스를 활용하시면 S3 데이터를 대상으로 시큐리티 사고 탐지도 가능합니다.

라고 얼마전까지는 GuardDuty 와 함께 Macie 에 대해서도 함께 소개하기도 했었는데요, 최근에 GuardDuty 의 기능이 대폭 확대되어 S3 Protection 기능을 추가로 설정하실 수도 있습니다!

S3 Protection 블로그 주소] https://aws.amazon.com/ko/blogs/aws/new-using-amazon-guardduty-to-protect-your-s3-buckets/

![image](https://user-images.githubusercontent.com/62640332/156537443-c1a30e79-ef6b-4d23-a900-d6e68f13fffa.png)

Usage 메뉴에서는 단순히 지금까지 GuardDuty 에서 사용한 비용을 보여줍니다. 실제로 활성화시켜보시면 아시겠지만, 대부분의 경우 많아봐야 전체 AWS 비용의 1~2% 를 차지할 정도의 소액입니다. 대신 시큐리티 사고에 대해 즉각적으로 통보를 받고 대응할 수 있도록 도와준다는 점에서 사실상 모든 AWS 어카운트에 무조건 활성화시켜둬야하는 서비스 중에 하나라고 생각합니다.

![image](https://user-images.githubusercontent.com/62640332/156537535-166962bc-8725-41b2-a37b-eca219f40b4e.png)

이번엔 Settings 메뉴로 넘어가보겠습니다. 빨간색으로 표시된 부분은 제가 강조하고 싶은 부분인데요, Findings 의 업데이트 주기는 기본값으로 6시간으로 설정되어있는데, 이 기본값을 가장 짧은 단위인 15분 으로 변경하시는 것이 좋습니다. 시큐리티 문제가 발생하면 다 막대한 비용을 치뤄야하는 경우가 흔하므로 당연히 조금이라도 빨리 Findings 의 통보를 받는것이 좋겠죠!

![image](https://user-images.githubusercontent.com/62640332/156537621-2397e024-9eac-43db-93dc-09b1c6d32c41.png)

Sample Findings 를 생성해주는 기능이 있습니다.

mazon GuardDuty 팀에서 학습목적 등으로 활용할 수 있는 Sample Findings 를 즉시 생성해주는 유용한 기능도 제공

![image](https://user-images.githubusercontent.com/62640332/156537813-8fcd1134-3871-43c8-975d-b1a82f734cea.png)

이번엔 List 메뉴입니다. GuardDuty 입장에서는 어떤 IP 가 위협을 줄 수 있는 IP 인지 모르기때문에 이러한 Trusted IP lists 와 Threat lists 를 설정해주는 것으로 불필요한 탐지와 통보를 막을 수 있습니다.

![image](https://user-images.githubusercontent.com/62640332/156537893-c7f42938-fbec-479d-88d2-1e727900107b.png)

다음은 최근에 출시된 S3 Protection 기능입니다! Amazon GuardDuty 를 활성화시켰더라도 이 기능만큼은 별도로 따로 활성화를 시켜줘야하므로 필요한 경우에는 이 기능을 직접 활성화해줍시다!

![image](https://user-images.githubusercontent.com/62640332/156538000-a7465f11-d8fd-4589-a3ee-f4950713677c.png)

이번엔 Accounts 메뉴입니다. AWS 를 사용해서 서비스를 구성하다보면 다양한 이유로 어카운트를 나누는 것을 고려하게 됩니다. 

Amazon GuardDuty 를 활성화시키고 제대로 활용하기 위해서는 모든 AWS 어카운트 의, 모든 리젼 에 GuardDuty 를 활성화시켜줄 필요가 있는데요,

활성화와 함께 위에서 설명한 Trusted IP lists / Threat lists 관리, 각종 설정 등을 하나하나 설정해줘야하는 번거로움들이 있습니다. 

이러한 불편함을 개선하기위해 마스터 및 멤버 계정을 통해 이러한 작업을 손쉽게 할 수 있도록 도와줍니다.

사실, 이 기능을 제대로 이해하기 위해서는 AWS Organizations 에 대해 이해할 필요가 있습니다.

https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_accounts.html#invitation_method


이번에는 Sample Findings 로 생성한 Finding 하나를 골라서 어떤 정보들을 알 수 있는지 간단히 살펴보겠습니다.

![image](https://user-images.githubusercontent.com/62640332/156538228-190b8b43-42c9-414c-be1e-9e903f898e5c.png)

Amazon GuardDuty 에서는 Finding 의 등급을 Low, Medium, High 의 3가지로 분류해줍니다. 색깔을 보시면 High 가 빨간색, Low 가 파란색입니다.

빨간색으로 표시된 Finding 중에 Backdoor:EC2/DenialOfService.Udp 라고 하는 종류의 Finding 을 살펴보겠습니다. 

참고로, GuardDuty 에서 제공해주는 Finding 의 모든 분류는 아래의 공식 도큐먼트에서 확인하실 수 있습니다.

https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-active.html

Resource Affected 항목에 기재된 리소스에 즉각적인 조치가 필요해보입니다. 아래에 좀 더 스크롤을 내려보면

![image](https://user-images.githubusercontent.com/62640332/156538462-1d338075-68a1-477d-82d7-fb5dbefa9cb8.png)

![image](https://user-images.githubusercontent.com/62640332/156538487-a9f15193-dfe0-4e47-b2d1-6c9fbd190ca8.png)

네트워크 정보나 좀 더 구체적인 액션의 정보 등을 확인하실 수 있습니다.

네트워크 정보나 좀 더 구체적인 액션의 정보 등을 확인하실 수 있습니다.

음..? 이게 GuardDuty 의 기능 전부냐구요?

네, 이게 전부입니다. Amazon GuardDuty 가 좋다는 건 많이 들어봤는데, 어떤 서비스인지 도무지 감이 안오시는 분들은, 바로 Amazon GuardDuty 의 역할이 검출까지 라는 전제를 파악하지 못하고 있을 가능성이 높습니다. 

그래서, 추가적인 조치나 대응, 복구 등을 위해서는 직접 대응하거나, 아니면 AWS 에서 제공하는 다른 서비스들을 다같이 조합해서 활용하시는 것이 권장됩니다.

하지만 Amazon GuardDuty 가 해주는 그 "Detect(검출)" 이라는 기능 자체는 굉장히 편리하고 유용합니다. 

대부분의 경우 시큐리티 사고가 발생했다는 것을 알게되는 순간은, 월별 청구서에 이상하리만큼 높은 비용이 청구되었을 때 액세스키가 누출된 것을 깨닫게되거나 혹은

아예 시큐리티 사고가 일어난줄도 모르고 있다가 우연히 알게되었을때는 이미 대응이 한참 늦게되는 상황이 벌어지게됩니다.

따라서 Amazon GuardDuty 의 역할은 시큐리티 사고를 "Detect" 하는 것까지 라고 이해를 하시면 Amazon GuardDuty 를 더욱 유용하게 사용하실 수 있으실거라 믿습니다.

Amazon GuardDuty 를 실전에서 활용하기 위한 가장 필요한 기능!! 바로 Findings 의 업데이트 정보를 즉각적으로 수신할 수 있도록 설정하는 기능인데요, CloudWatch Events 를 통해 설정하실 수 있습니다.



