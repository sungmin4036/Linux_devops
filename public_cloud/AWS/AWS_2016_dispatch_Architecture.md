ㅁ 디스패치 AWS 사용 이유

- 특종 기사가 올라올시, 트래픽 처리 어려움
- 외주를 통한 기존 데이터센터 인프라 운영으로 비용 증가(사용하지도 않는대도 서버를 확보해야 하다보니 돈이 많이듬)


![image](https://user-images.githubusercontent.com/62640332/155708944-20368faa-9033-443e-80af-8d49fecbe8ab.png)

이 사진은 평상시 트래픽을 나타내는 사진.

![image](https://user-images.githubusercontent.com/62640332/155709084-647a13f0-334c-4a71-b3ac-f2fc56a5ebcb.png)

위 사진은 특종기사 나올때 트래픽 나타내는 사진 으로, 평소보다 10배이상의 트래픽이 발생하게 됩니다. 

![image](https://user-images.githubusercontent.com/62640332/155709219-66e30006-90c2-4dda-8b92-7fc5cfb9a9c5.png)

x는 시간 y는 인스턴스로 10월달 부터는 인스턴스 타입 최적화를 하였습니다.

![image](https://user-images.githubusercontent.com/62640332/155709319-ba207bb5-accf-467b-b4ad-44085822254f.png)

처음 도입했을때 구조.

웹서버를 오토스켈링으로 구성했고, 가운데만 오토스켈링을 하였습니다.

![image](https://user-images.githubusercontent.com/62640332/155709470-8585f95a-f3f7-408b-ab48-0021c4605cd6.png)

동시처리에 유리한 Nginx로 교체, 아마존 사용 장점중 하나로 필요시 인스턴스 설치하여 서비스 올리고, 기존 인스턴스 정지후 빼내어 

무정지로 처리 하였습니다.

![image](https://user-images.githubusercontent.com/62640332/155709705-52342604-1f1e-4d57-a239-81e58113adcf.png)

ElasticCache를 웹서버와 DB서버에 적용해서 트래픽을 분산시켰습니다. 기존에도 파일캐시를 적용했지만, 파일캐시 특성상

웹서버간에 캐시를 공유하기 어렵습니다. 예를들어 서버가 10대라면 10대의 캐시를 설치하는 시간가 비용이 듭니다.

하지만 elesticCache를 사용하여 서버간 캐시를 공유해서 시간과 비용 줄이기 가능해졌습니다.

![image](https://user-images.githubusercontent.com/62640332/155709918-a01c495e-4029-4793-ab80-184843c8e9b5.png)

이렇게 효율적으로 되면서, 인스턴스 타입도 다운그래이드 하여 운영이 가능해졌습니다.

위는 웹서버 인스턴스, 아래는 DB 인스턴스 변경한것 의미하며,

![image](https://user-images.githubusercontent.com/62640332/155710062-82be4b66-8dcb-4e2a-9ad3-e1cbd640b55c.png)

전제척인 인스턴스 량도 줄어들어서 비용이 가능해 졌습니다.

여기에 예약 인스턴스와 스팟 인스턴스를 추가하면 더욱더 저렴하게 이용가능합니다.

