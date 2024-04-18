
# devops

전체적인 데브옵스 내용 정리와 기초 지식

ㅁ 추가적으로 알면 좋은것

[- YAML문법](https://subicura.com/k8s/prepare/yaml.html#%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8)<br>
[- docker docs](https://docs.docker.com/)<br>
[- kakao ent container deep1](https://tech.kakaoenterprise.com/154)<br>
[- kakao ent container deep2](https://tech.kakaoenterprise.com/171)<br>
[- debug 기초](https://hopeis.tistory.com/270)<br>
[- k8s 기초](https://happycloud-lee.tistory.com/246)<br>
[- devops 겉](https://www.devkuma.com/docs/category/dev-ops/)<br>
[- k8s etc 자세한 원리](https://tech.kakao.com/2021/12/20/kubernetes-etcd/)<br>
[- cgroup 내용추가](https://tech.kakao.com/2020/06/29/cgroup-driver/)<br>

---

set -o errexit 또는 set -e: 이미 설명한 대로, 명령이 실패할 때 스크립트가 즉시 종료됩니다.

set -o nounset 또는 set -u: 이 옵션을 활성화하면, 사용되지 않은 변수를 사용하려고 시도할 때 스크립트가 실패합니다. 이는 변수를 정의하지 않고 사용하는 오류를 방지하는 데 도움이 됩니다.

set -o pipefail: 이 옵션을 활성화하면 파이프 (|)로 연결된 명령 중 하나라도 실패하면 전체 파이프 명령이 실패합니다. 기본적으로 파이프 명령은 마지막 명령의 종료 상태만 반환합니다.

set -o noclobber: 이 옵션을 활성화하면 파일 덮어쓰기를 방지합니다. 즉, > 나 >> 등을 사용하여 파일을 작성할 때 이미 존재하는 파일을 덮어쓸 수 없게 됩니다.

set -o noglob 또는 set -f: 이 옵션을 활성화하면 파일 이름 확장 (globbing)을 비활성화하며, 와일드카드 문자(예: * 또는 ?)를 일반 문자로 처리합니다.

set -o xtrace 또는 set -x: 이 옵션을 활성화하면 스크립트가 실행하는 각 명령을 출력하므로 디버깅에 유용합니다. 스크립트를 실행하기 전에 명령이 표시됩니다.

set -o verbose 또는 set -v: 이 옵션을 활성화하면 스크립트가 실행하는 각 명령을 출력하지만, xtrace와 달리 명령이 실행되기 전에 표시됩니다.

set -o ignoreeof: 이 옵션을 활성화하면 Ctrl-D를 사용하여 터미널에서 스크립트를 종료하는 것을 방지합니다. 이것은 실수로 스크립트를 종료하지 않도록 하는 데 도움이 됩니다.

---

```
sh test.sh == ./test.sh   <--- sub shell 생성
. test.sh == source test.sh   <--- sub shell 생성X
```


---

```
$0, $1, $2, ...:

$0은 스크립트의 이름을 나타내며, $1, $2, 등은 스크립트에 전달된 인수(매개변수)를 나타냅니다.
예를 들어, 스크립트를 ./myscript.sh arg1 arg2로 호출하면, $0는 myscript.sh, $1은 arg1, $2는 arg2가 됩니다.
$#:

$#는 매개변수의 개수를 나타냅니다.
$@:

$@는 모든 매개변수를 나타냅니다. 각 매개변수는 따옴표로 묶이지 않습니다.
$*:

$*는 모든 매개변수를 나타냅니다. 이러한 매개변수는 하나의 문자열로 결합됩니다.
$?:

$?는 이전 명령의 종료 코드(Exit Code)를 나타냅니다. 일반적으로 0은 성공을 나타내며, 0이 아닌 값은 실패를 나타냅니다.
$$:

$$는 현재 스크립트의 프로세스 ID (PID)를 나타냅니다.
$!:

$!는 가장 최근에 백그라운드에서 실행된 프로세스의 PID를 나타냅니다.
${variable:-default}:

${variable:-default}는 변수가 정의되어 있지 않으면 기본값(default)을 사용합니다.
${variable:=default}:

${variable:=default}는 변수가 정의되어 있지 않으면 변수에 기본값을 할당합니다.
${#variable}:

${#variable}는 변수의 길이를 나타냅니다. 이것은 문자열의 문자 수 또는 배열의 요소 수를 세는 데 사용됩니다.

${#variable}는 변수의 길이를 나타냅니다.

${#variable[@]}는 배열 변수의 요소 수를 나타냅니다.

```

---

IFS=$'\n' read -rd '' -a lines <<< "$pending_line"
current_pending_lines="${#lines[@]}"

IFS=$'\n':

IFS는 "Internal Field Separator"의 약자로, Bash에서 텍스트를 분리하는 데 사용되는 구분자입니다.

이 줄은 IFS를 줄 바꿈 문자($'\n')로 설정합니다. 이것은 입력 문자열을 줄 바꿈 문자를 기준으로 분리하게 됩니다.

---

read -rd '' -a lines:

read 명령어는 표준 입력으로부터 데이터를 읽고 변수에 할당하는 데 사용됩니다.

-r 옵션은 이스케이프 시퀀스(escape sequence)를 해석하지 않도록 합니다.

-d '' 옵션은 빈 문자열을 구분자로 사용하도록 지정합니다. 이것은 구분자를 사용하지 않는다는 의미입니다.

-a lines는 읽은 값을 배열 lines에 저장하라는 것을 의미합니다.

---

<<< "$pending_line":

<<< 연산자는 문자열을 표준 입력으로 전달하는데 사용됩니다.
$pending_line 변수의 내용을 문자열로 취급하여 read 명령어에 전달됩니다.

---

- 단어 찾기
awk -F'/' '/Stream:/ {print $4}

- 앞뒤 공백 제거
tr -d '[:space:]'


```
| 파이프 처리는 subshell 생성해서 처리하는것.

#subshell 에서 생성하다보니 변수 할당을 해도 전달이 불가능.
echo "hello world" | read first second
echo $second $first

# <<< 같은 프로세스 에서 처리하여 변수 할당 위해서는 아래의 방법사용
read first second <<< "hello world"
echo $second $first
```
