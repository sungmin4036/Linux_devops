- cgroup이란?

: 프로세스 할당하는 cpu, 메모리, 네트워크 대역폭 등과 같은 자원을 개별적으로 제한하기 위한 시스템 커널의 기능

컨테이너 런타임은 이 기술을 사용하여 컨테이너에 자원을 할당.

cgroup을 관리하기 위한 기술은 크게 `cgroupfs` 와 `systemd` 가 있음.

cgroup이 관리할 수 있는 자원의 컨트롤러는 파일 시스템의 형태로 관리되고 있음.

이 때 이와 관련된 정보를 파일 시스템의 형태로 변환시켜 주는 매니저가 `cgroupfs`

![image](https://user-images.githubusercontent.com/62640332/167880891-f4f21959-5dbe-4e3f-a429-7cc6ef04a606.png)


![image](https://user-images.githubusercontent.com/62640332/167880959-2449f8eb-750d-4b3c-91de-dc6b82e99a83.png)

### ㅁ systemd 와 cgourp의 차이점

- cgroup

: cgourp(v1) 디렉터리 하위에서 리소스를 직접 매핑하는 구조를 가진다.


![image](https://user-images.githubusercontent.com/62640332/167880614-a038a93a-bbdb-4ee1-9a71-b1524decf8c6.png)

<br>

- systemd

: systemd는 프로세스가 사용하는 자원을 관리하기 위해 slice > scope > service 단위의 계층 구조를 만들어 각 단위에 자원을 할당.

이러한 계증 구조는 systemd-cgls 명령어가 표현해주는 구조 혹은 /sys/fs/cgourp/systemd 하위의 파일 시스템의 계층 구조를 통해서 확인 가능.

이 떄 systemd 에 의해 관리되는 cgroup과 관련된 정보는 /sys/fs/cgroup 하위의 자원의 컨트롤러(cpu, memory)등의 하위에서 slice의 이름으로 된 파일 시스템으로 표현


![image](https://user-images.githubusercontent.com/62640332/167881538-825f9863-a907-461d-b95a-0f40e1b9fd5b.png)

<br>

### ㅁ cgourp에 대한 쿠버네티스와 도커 선호도 차이는 왜?

이론적으로 직접 cgorup을 마운트해서 사용하는 cgroupfs을 이요하거나 systemd와 같은 계층적 구조로 만들어 접근하거나 결과적으로 동일해야 한다.

하지만 쿠버네티스에서는 systemd를 선호하고, 도커에는 cgroupfs를 선호하지만 이유는 못찾음.

다만 쿠버네티스 공식 문서에서는 도커 18.03 + 커널 4.17.17 이하의 RHEL 에서는 cgroupfs에서 메모리 leaking issue가 발생하여 systemd 변경하로 권고

도커에서는 cgroupfs를 cgroup v1에서만 사용하고, cgorup v2에서는 systemd로 변경할 계획 밝힘.

결과적 으로는 cgroup을 구조화하는 방법론적 차이가 있는 수준으로 추정


![image](https://user-images.githubusercontent.com/62640332/167882660-cbba3ce7-71b1-4e10-a7da-5ed3b73cfcd4.png)



