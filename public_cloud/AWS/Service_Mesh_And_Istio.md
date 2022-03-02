- [들어가며](#들어가며)
  
- [Service Mesh](#ㅁ-service-mesh)
  
- [클라우드 네이티브(Cloud Native)와 쿠버네티스(Kubernetes)의 등장](#ㅁ-클라우드-네이티브(Cloud-Native)와-쿠버네티스(Kubernetes)의-등장)

- [서비스 메쉬의 핵심, 사이드카(Sidecar) 패턴](#서비스-메쉬의-핵심,-사이드카(Sidecar)-패턴)


- [부하분산과 트래픽 관리](#ㅁ-부하분산과-트래픽-관리)

- [보안](#ㅁ-보안)

- [모니터링](#ㅁ-모니터링)

- [서비스 메쉬 적용 시 고려사항](#ㅁ-서비스-메쉬-적용-시-고려사항)

- [이스티오(Istio)](#ㅁ-이스티오(Istio))
---

#### 들어가며

최근 많은 기업이 기존 모놀리식 아키텍처(Monolithic Architecture)의 한계를 극복하고 클라우드 환경에서 시스템 운영 이점을 극대화하기 위해 마이크로서비스 아키텍처(Microservices Architecture, 이하 MSA)를 채택하고 있습니다.

현재 뜨거운 관심을 받고 있는 MSA는 새로운 개념으로 봐야 할까요? 그렇지 않습니다. 

15년 전에 지금의 MSA와 유사한 SOA(Service Oriented Architecture)라는 개념이 소개되었기 때문입니다. 

SOA는 애플리케이션을 비즈니스적인 의미를 가지는 기능 단위로 묶어서 표준화된 호출 인터페이스를 통해 서비스라는 소프트웨어 컴포넌트로 재조합하여 업무를 구현하는 아키텍처로 당시에는 획기적인 사상이었습니다.

SOA는 실패했을까? 그럼 MSA는?

SOA는 비즈니스 로직에 집중하고 도메인을 중심으로 서비스를 분리하는 등 MSA와 여러모로 유사합니다.

반면 SOA는 분산처리 환경에서 발생하는 문제를 해결하기 위해 ESB(Enterprise Service Bus)를 사용했다는 점이 MSA와 다릅니다. 

당시에는 장밋빛 미래가 보장될 것만 같았던 SOA는 결국 실패한 것으로 평가받았습니다. 그 이유는 다양하게 거론되지만 여기서는 두 가지만 언급하도록 하겠습니다.

- 첫째, 조직 변화 수용의 실패

“모든 시스템은 그 조직의 의사소통 구조와 동일하게 만들어진다”는 콘웨이의 법칙(Conway's law)은 1968년 미국의 컴퓨터 과학자인 멜빈 콘웨이가 주장한 것으로 팀을 구성하는 방법이 업무 수행 방식에 영향을 미친다는 이론입니다. 

SOA는 조직에 엄청난 변화를 가져오기에 제대로 적용하기 위해서는 미지의 두려움에 대한 도전이 필요했습니다. 

SOA 자체가 복잡한 데다 기능을 전문화하기 위해서는 무엇보다 조직 간 협업이 필수적이었습니다. 

하지만 조직의 인식이 이런 개념을 수용하지 못했고 조직이 변화해야 한다는 사실도 인지하지 못하였습니다.

최근 클라우드 컴퓨팅이 부상하면서 대두된 데브옵스(DevOps)와 SRE(Site Reliability Engineering)는 조직의 변화가 수반되어야 함을 강조합니다. 

MSA도 마찬가지입니다. MSA를 성공적으로 적용하기 위해서는 조직의 구조 역시 이를 수용할 수 있는 형태로 바뀌어야 합니다.

<br>
<br>

-  둘째, ESB의 부하 중앙 집중 문제

분산 처리 환경의 문제를 애플리케이션 외부에서 해결하려고 한 것이 ESB와 여기서 소개하려는 서비스 메쉬(Service Mesh)의 유사점이라고 할 수 있습니다. 

하지만 ESB는 중앙집중형으로 공통 기능의 비대화에 따른 문제를 제대로 해소하지 못하였습니다. 

SOA라는 개념은 상당히 복잡해서 그 이상에 비해 당시의 기술력이 부족했던 점도 실패 원인 중 하나로 들 수 있습니다.

SOA가 시들해진 지 수 년이 지났고 다시 MSA라는 개념이 화두가 되고 있습니다. 그럼 현재의 MSA는 과거의 SOA와 무슨 차이점이 있는 것일까요?

MSA와 SOA는 무엇이 다를까?

SOA의 ESB는 중앙에 위치하여 서비스 간 통신을 담당합니다.

![image](https://user-images.githubusercontent.com/62640332/156036058-311311c5-4f2c-4c5b-ac8e-490bf553892b.png)

MSA 개념에서 보면 [그림 2]와 같이 하나의 ESB를 다수의 마이크로서비스 컴포넌트로 분리하여 처리하는 것과 다르지 않습니다.

![image](https://user-images.githubusercontent.com/62640332/156036096-bf58299b-55d8-468c-b9a0-111632cea1aa.png)

ESB를 활용하는 SOA와 달리 마이크로서비스 아키텍처는 서비스 간 통신 기능을 구현하는데 많은 시간과 노력을 들여야 한다는 것을 짐작할 수 있을 것입니다. 

사실 MSA에서 가장 어려운 부분은 서비스 자체를 구축하는 것이 아니라, 서비스 간의 통신을 처리하는 것입니다. 

통신 흐름을 제어하는 것이 대단히 복잡하기 때문입니다. 그래서 필요한 것이 바로 서비스 메쉬입니다.

본 아티클에서는 서비스 메쉬의 등장 배경과 이를 구현한 오픈소스 솔루션인 이스티오(Istio)에 대해 알아보도록 하겠습니다.

<br>
<br>
---

#### ㅁ Service Mesh
서비스 메쉬와 넷플릭스 OSS

하나의 애플리케이션에서 동작하는 모놀리식 아키텍처와 달리 MSA는 나누어진 서비스와 인스턴스 수만큼 관리 대상이 급격히 증가합니다. 

또한 서비스 간 복잡한 연결 구조 때문에 장애 추적이 어렵고, 장애가 발생한 서비스로 인해 타 서비스를 호출하는 로직이 수행되지 않으면서 다른 서비스까지 동작하지 않는 장애 전파 현상이 나타납니다. 

서비스 메쉬 아키텍처는 MSA의 트래픽 문제를 보완하는 마이크로서비스 간 커뮤니케이션 인프라입니다. 

서비스 메쉬를 사용하면 마이크로서비스 간에는 직접 통신을 하지 않게 됩니다. 

대신 모든 통신은 애플리케이션 네트워크 기능을 제공하는 서비스 메쉬를 통해 이루어집니다.

클라우드에 적극적이었던 넷플릭스(Netflix)는 이런 탄력적인 서비스의 필요성을 가장 먼저 깨닫고 서킷 브레이커 패턴의 Hystrix, 서비스 디스커버리 패턴의 Eureka, 모니터링 서비스인 Turbine 등의 컴포넌트를 개발하여 넷플릭스 OSS(Open Source Software)로 공개하였습니다. 

아울러 오픈소스 애플리케이션 개발 프레임워크인 스프링 프레임워크(Spring Framework)에 넷플릭스 OSS를 통합한 Spring Cloud Netflix를 제공하여 손쉽게 마이크로서비스를 구현할 수 있도록 하였습니다. 

하지만 넷플릭스 OSS는 Java로 개발된 관계로 이를 사용하기 위해서는 라이브러리를 포함하고 코드에 관련된 컴포넌트를 추가해야 하는 등의 제약이 있습니다. 

또한, 당시에는 가상머신이 클라우드에서 애플리케이션을 실행하는 유일한 방법이었고 넷플릭스 OSS도 이에 최적화되어 있다 보니 최신의 운영 환경을 모두 수용하지 못하는 단점이 있습니다.

<br>
<br>

##### ㅁ 클라우드 네이티브(Cloud Native)와 쿠버네티스(Kubernetes)의 등장

현재 많은 기업이 기존의 레거시 시스템을 클라우드 환경으로 이전하고 있습니다. 

이와 같이 클라우드 컴퓨팅의 이점을 활용하는 애플리케이션 구축 및 실행 컨셉을 클라우드 네이티브(Cloud Native)라고 하며, 이는 클라우드를 기반으로 개발 생산성과 IT 속도를 극대화하기 위해 탄생하였습니다.

퍼블릭 클라우드 벤더(AWS, Microsoft, Google 등)의 성장과 함께 컨테이너 기술 개발도 활발해졌는데 가장 빠르게 발전한 것이 오픈소스 컨테이너 오케스트레이션 플랫폼인 쿠버네티스(Kubernetes)입니다. 

현재 쿠버네티스와 컨테이너는 마이크로서비스 애플리케이션을 실행하는데 사실상 표준이 되었습니다. 

다수의 컨테이너에 애플리케이션 서비스를 올리게 되면서 수많은 애플리케이션을 운영하는 환경으로 변화되었습니다. 

컨테이너 관리와 더불어 마이크로서비스로 불리는 서비스 간의 통신 역시 증가할 수밖에 없게 되었고, 이로 인해 네트워크와 밀접한 애플리케이션 기능인 서비스 메쉬에 대한 관심도 높아지는 형국입니다.

###### 서비스 메쉬의 핵심, 사이드카(Sidecar) 패턴

사이드카(Sidecar) 패턴은 모든 응용 프로그램 컨테이너에 사이드카 컨테이너를 추가하여 배포합니다. 

사이드카는 서비스에 들어오거나 나가는 모든 네트워크 트래픽을 처리합니다. 

가장 큰 특징은 비즈니스 로직이 포함된 실제 서비스와 사이드카가 병렬로 배치되기 때문에 서비스가 타 서비스를 직접 호출하는 것이 아니라 프록시(proxy)를 통해서 호출한다는 점입니다. 

따라서 대규모의 마이크로서비스 환경이더라도 별도 작업 없이 서비스 연결뿐만 아니라 로깅, 모니터링, 보안, 트래픽 제어가 가능하다는 장점이 있습니다.

![image](https://user-images.githubusercontent.com/62640332/156036140-df7d32e7-3860-4ef8-9fc6-40c31afa1aec.png)




##### ㅁ 부하분산과 트래픽 관리

이스티오는 파일럿과 사이드카 프록시인 Envoy를 통해 트래픽을 제어하고 관리합니다.

![image](https://user-images.githubusercontent.com/62640332/156036230-3a4cd121-86f2-4562-9f62-67a20bfaa982.png)

믹서는 파일럿을 이용하여 서비스 버전별 트래픽 양을 분산하거나 요청 콘텐츠에 따라 특정 버전별로 트래픽을 분할하여 전송할 수 있습니다. 

또한, 파일럿을 통해 서비스를 디스커버리하고 로드밸런싱합니다. 

서비스 상태를 주기적으로 체크하여 비정상적인 인스턴스는 자동으로 제거합니다. 

서비스 간 호출 안전성을 위해 호출 재시도 횟수를 통제하거나 응답 시간에 따른 에러 처리 등 통신 상태도 관리할 수 있습니다.

##### ㅁ 보안

이스티오는 모든 서비스의 트래픽이 Envoy를 통해 이뤄지며 TLS(Transport Layer Security)를 이용하여 서비스 간 통신이 암호화됩니다. 

TLS 통신을 위한 인증서와 키는 시타델에서 관리합니다. 

이스티오는 여러 가지 인증 방식을 제공하고 있습니다. 

서비스 간 인증, 서비스와 엔드 유저(클라이언트) 간 인증이 가능하고 역할 기반 접근 제어(RBAC, Role Based Authentication Control) 기능으로 서비스 접근 권한을 통제합니다. 

인증된 서비스나 엔드 유저에게는 특정 역할을 부여하고 이에 기반하여 서비스 접근이 가능하도록 합니다.

![image](https://user-images.githubusercontent.com/62640332/156036261-1f15caf5-0cff-4766-b6bd-421c9616d964.png)

##### ㅁ 모니터링

이스티오는 믹서를 통해 서비스 간 호출 관계, 서비스 응답시간, 처리량 등의 네트워크 트래픽 지표를 수집하고 모니터링합니다. 

모든 서비스는 호출될 때 앞단의 Envoy를 거치기 때문에 각 서비스의 호출 시 또는 주기적으로 믹서에 정보를 전달하고 이를 로깅할 수 있습니다. 

믹서는 손쉽게 플러그인(Plugin)이 가능한 어댑터(Adapter) 구조로 여러 모니터링 시스템에 연결됩니다. 

예를 들면 쿠버네티스에서 많이 사용하는 Heapster, Prometheus, StackDriver, Datadog 등의 모니터링 도구들과 연계하여 시각화할 수 있으며 Grafana를 통해 각 서비스의 응답시간, 처리량 등을 확인할 수 있습니다. 

또한 Jaeger를 이용하면 분산 트랜잭션 구간별로 응답 시간을 모니터링할 수도 있습니다.

##### ㅁ 서비스 메쉬 적용 시 고려사항

앞서 언급했듯이 MSA의 가장 큰 화두는 마이크로서비스 간의 통신 흐름으로 볼 수 있습니다. 이 관점에서 서비스 메쉬도 적용 시 고민해야 할 부분이 존재합니다.

   - 복잡성: 서비스 메쉬를 사용하면 런타임 인스턴스 수가 증가합니다.
   - 사이드카 컨테이너 수 증가: 각 서비스는 서비스 메쉬의 사이드카 프록시를 통해 호출되므로 개별 프록시 수가 증가하게 됩니다. 이에 따른 부하로 서비스 운영에 문제가 발생할 가능성이 있는지에 대해 아키텍처 구조 측면에서 사전에 검토해야 합니다.
   - 기술력의 미성숙: 이스티오는 빠르게 발전하고 있으나 아직은 새롭고 미성숙한 기술에 속합니다. 또한 많은 기업이 서비스 메쉬에 대한 경험이 없는 실정입니다.

##### ㅁ 이스티오(Istio)
쿠버네티스 기반의 서비스 메쉬, 이스티오

서비스 메쉬의 기본 아키텍처는 서비스 간 직접 호출 대신 앞에서 언급한 경량화된 사이드카 패턴의 프록시를 배치하여 통신합니다. 

이런 서비스 메쉬 기술의 대표적인 구현체가 이스티오입니다. 

Google, IBM, Lyft 등이 참여하는 오픈소스 프로젝트로 2018년 일반에 공개된 이스티오는 Spring Cloud Netflix와 많은 유사점이 있습니다. 

하지만 Java에 국한된 Spring Cloud Netflix와 달리 플랫폼 영역에서 동작하기 때문에 개발 언어와 무관하게 사용할 수 있습니다.

무엇보다 쿠버네티스와 클라우드 기반에 적합하여 이미 Red Hat OpenShift, VMware Tanzu 등 PaaS 플랫폼의 서비스 메쉬로 선택되기도 하였습니다. 

컨테이너 환경에서 MSA 상의 분산 아키텍처를 수용하기 위한 노력이 더해지면서 이스티오에 대한 세간의 관심이 높아지는 추세입니다.

![image](https://user-images.githubusercontent.com/62640332/156036189-4fecb61b-2478-484c-aaa0-b8facf7ab960.png)



MicroService Architecture 시스템의 서비스간 통신이 Mesh Network 형태를 띄는것에 빗대어 명명
MSA 시스템이 커지고 서비스가 점점 증가하고 관리가 어려워지고.. 통신, 메트릭, 보안 등등 서비스간의 상호 동작이 복잡해지고 있다.

### A. 서비스가 몇개더라?

- 몇개의 서비스가 있지?
- 서비스 당 인스턴스는 몇개나 존재하지?
- 서비스 인스턴스에 로드밸런싱은 어떻게 하지?
- 잘 떠있나?

### B. 저기서 장애가!

A -> B -> 외부 API

- 외부 API 장애 start
- B: 타임아웃 예정 스레드 증가
- A: 그에 따른 B에 대한 타임아웃을 기다리는 스레드 증가

어딘가에서 A를 호출하고 있다면? 쾅~ 장애 전파

### 문제 해결

A를 풀자 - Service Discovery Pattern

- Registry에서 각 서비스 인스턴스 정보 관리
- 인스턴스 정보를 꺼내서 로드밸런싱!
- Netflix Eureka + ribbon 조합


B를 풀자 - Circuit Breaker Pattern

- 일정 수치 이상 에러 발생 -> fallback method 실행
- 장애 전파 차단 및 전체 시스템 보호
- Netflix Hystrix

이정도면 ?
위 해결법을 적용하면 꽤 많이 좋아질 수 있다. 그러나

이것들을 적용하려면 개발자가 다 해야한다.

결국 내 애플리케이션이 위를 지원해야하고, 또 다른 애플리케이션도 모두 지원되어야 한다.

내 바람대로 동작 할 소프트웨어가 전부 구축 되어야 한다.

이를 인프라 레벨에서 컨트롤 할 수 있다면 좋지 않을까

즉 Service Mesh란, software적 접근이 아닌 인프라 레벨에서 서비스간 통신 및 그 외 여러 부분들을 담당하고 관리하자는 컨셉이다. 그럼 어떤식으로 인프라 레벨에서 이를 관리할까

#### Proxy

```
프록시(Proxy)란 "대신" 이라는 의미를 가지고 있습니다. 프로토콜에 이썽서 대리 응답 등에서 사용하는 개념으로, 보안상의 문제로 직접 통신을 주고 받을 수 없는 사이에서
프록시를  이용해서 중계를 하는 개념
```

Service Mesh 컨셉은 서비스 간 호출의 경우 / 서비스간 라우팅의 경우 직접 호출하는 것이 아닌 프록시를 두고 아래와 같이 진행한다.

![image](https://user-images.githubusercontent.com/62640332/156369706-5590495c-3f3d-4282-82e4-b3c1199b96d7.png)

이를 통해 각 서비스로 들어오는 트래픽을 proxy가 통제하고 차단 할 수 있다. 위에서 B상황의 경우 circuit breaking을 이 proxy가 행할 수 있다.

#### Data Plane & Control Plane

서비스의 증가하면 프록시 수도 증가하게 된다. 프록시 수의 증가는 또다시 관리 포인트에 대한 비용을 생성한다.

이 문제를 해결하기 위해 각 프록시에 대한 config 정보를 중앙에서 컨트롤 하는 구조를 취해야한다.

![image](https://user-images.githubusercontent.com/62640332/156370087-01ad6d41-6a3d-4deb-ba48-c735193203de.png)



Data Plane은 각 프록시들로 이루어져 config에 따라 트래픽을 컨트롤 하는 부분을 말하고,

Control Plane은 Data plane의 config 값들을 저장하고, 프록시들에 이를 전달하는 컨트롤러 역할을 한다.

#### Envoy Proxy
Istio는 envoy 프록시를 사용한다. 이는 Lyft사에 의해 개발 되었으며 다양한 기능을 제공한다.

- TCP, HTTP1, HTTP2, gRPC protocol 지원
- TLS client certification
- L7 라우팅 지원 및 URL 기반, 버퍼링, 서버간 부하 분산량 조절
- Auto retry / Circuit Breaker 지원 / 다양한 로드밸런싱 기능 제공
- Zipkin을 통한 분산 트랜잭션 성능 측정 제공
- Dynamic configuration 지원, 중앙 레지스트리에 설정 및 설정 정보를 동적으로 읽어옴
- MongoDB에 대한 L7 라우팅 기능

서비스들은 Envoy proxy와 함께 Data Plane을 구성할 수 있다.

 

#### Istio
Envoy Proxy로 구성된 Data Plane을 컨트롤 하는 것이 Istio이다. k8s에서는 envoy proxy가 pod에 sidecar 형태로 배포된다. istio-sidecar-injector가 자동으로 istio(envoy) proxy를 pod에 주입한다.

 
istio architecture

![image](https://user-images.githubusercontent.com/62640332/156370218-67a9833a-cc34-47c4-991f-8b15a4af67d3.png)


istio의 core component는 아래와 같다.

- Envoy
- Pilot
- Citadel
- Gallery


#### Pilot
- Envoy sidecars를 위한 service-discovery를 제공
- Istio에 배포된 envoy 의 생명 주기를 담당하며, 각 envoy는 pilot으로부터 가져온 다른 인스턴스 정보들로 로드밸런싱을 하게 된다.
- Traffic Management API 를 사용해 Pilot이 envoy proxy가 더 세밀한 구성을 할 수 있게 도와준다.
  - traffic management capabilities for intelligent routing
  - retry, circuit breaker, timeout 등

![image](https://user-images.githubusercontent.com/62640332/156370342-d4de6f5a-e0c3-4c66-b882-1388275a452c.png)


- 새로운 인스턴스가 생성되면 platform adapter에게 알림
- 인스턴스가 생성 되었으니 트래픽 규칙과 구성 변경을 알리기 위해 envoy proxy들에 이를 알림

#### Citadel

- 보안에 관련된 기능을 담당하는 모듈
- 서비스를 사용하기 위한 사용자 인증 및 인가 담당
- Istio는 TLS를 이용해 통신할 수 있는데, 이때 certification을 관리하는 역할을 한다.


#### Gallery

- provides configuration management services for Istio.
 

#### Traffic Management

Istio는 pilot(service discovery)과 envoy proxy를 활용해 트래픽을 컨트롤 하게 된다.

pilot을 통해 envoy proxy는 트래픽을 관련 서비스로 전달 한다. 또한 기본적인 디스커버리와 로드밸런싱 외에 트래픽의 특정 비율을 새 버전의 Pod으로 보내는 등 더 많은 세밀한 규칙을 지정 할 수 있다.


아래 그림이 Istio의 Traffic Management feature이다.

![image](https://user-images.githubusercontent.com/62640332/156370488-ab72a4f7-7696-47e8-8409-8c86534d9d7e.png)


위 feature 들을 활용하려면 필수적으로 알아야 할 것들이 있는데 아래와 같다.

- Gateway
- Virtual Service
- DestinationRule

위 3개의 요소들을 더 이해하기 편하도록 도식도를 그렸다. 글을 다 읽고 보면 더 이해가 잘 될지도...?


![image](https://user-images.githubusercontent.com/62640332/156370575-8c9f2d59-bd93-4b89-b547-585d492828c1.png)

#### Gateway

Gateway는 외부로부터 트래픽을 받으며 가장 앞단에 존재한다. 

mesh에 대한 inbound 및 outbound traffic을 관리하여 mesh에 진입하거나 나갈 트래픽을 지정할 수 있다. gateway는 서비스에 붙는 envoy 처럼 sidecar 로 실행 되는 것이 아니고, standalone envoy proxy 형태로 생성된다.


Istio를 사용한다고 해서 외부에서 들어오는 트래픽을 반드시 Istio gateway를 사용해야 하는 것은 아니다.

기존의 Kubernetes Ingress를 그대로 사용할 수 도 있다.


#### Virtual Service
istio traffic routing 기능의 핵심 구성 요소이고, 이스티오의 트래픽 관리를 유연하게 만드는 핵심적인 역할을 한다.

virtual service는 k8s service로 요청이 라우팅 되는 방법을 구성할 수 있다. 또한 워크로드에 트래픽을 보내기 위한 다양한 트래픽 라우팅 규칙을 지정하는 방법들을 제공한다.

virtual service 가 없는 경우, envoy는 모든 서비스 인스턴스 간에 round robin 로드 밸런싱을 사용해 트래픽을 분산 시키다.

그러나 여기 모든 서비스에 virtual service를 적용시키 것이 best라고 한다.

client는 virtual service에 요청을 보내고, envoy는 virtual service rule에 따라 각기 다른 버전으로 라우팅 하는 형태이다. (ex. 20%는 새 버전, 80%는 이전 버전으로 라우팅)

이를 통해 새 서비스 버전으로 전송되는 트래픽의 비율을 점차적으로 늘릴 수 있다.(아래에서 다시 설명) 또한 http/1.1, http2, grpc, tcp 까지 라우팅 규칙을 구성할 수 있다.





[출처]

https://istio.io/docs/concepts/what-is-istio/
https://istio.io/docs/ops/deployment/architecture/
https://istio.io/docs/concepts/traffic-management/
https://istio.io/docs/reference/config/networking/destination-rule/
https://istio.io/docs/reference/config/networking/virtual-service/
https://istio.io/docs/reference/config/networking/gateway/

