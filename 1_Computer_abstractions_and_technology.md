# 1장. 컴퓨터 추상화 및 관련 기술



<br>

> 글쓴이: 이예찬  
> `컴퓨터 구조 및 설계`(`Computer Organization and Design - 5th edition; David A. Patterson, John L. Hennessy`)의 내용을 정리한 문서이다.  
> 컴퓨터 구조를 예습하면서, 나중에 이 내용이 필요할 때 다시 찾아보기 위해 문서화한다.  
> 누구든지 이 글을 공부 목적으로 사용하셔도 좋습니다.  
<br>



## 1.1 개론
<br>

### 컴퓨터의 종류
컴퓨터는 크게 세 가지 응용 분야에서 사용된다. 각 응용 분야에 따라 설계 요구사항과 하드웨어 기술을 사용하는 방법이 각기 달라진다.
- `personal computer`: 낮은 가격으로 단일 사용자에게 좋은 성능을 제공하는 것이 중요시됨
- `server`: 대형 작업 수행에 주로 이용됨. 연산과 입출력 용량의 확장성이 큼. 일반적으로 신용도(dependability)가 매우 강조됨. 저사양 서버에서부터 슈퍼컴퓨터까지 가격과 성능의 폭이 매우 넓음.
- `embedded computer`: 응용 분야와 성능이 매우 다양함. 최소한의 성능만 유지하면서 가격과 소모 전력을 엄격히 제한해야 하는 독특한 요구사항을 갖는 경우가 많음.
<br>

지금까지 우리는 업무 등에 PC를 사용하고 서버는 on-premise 서버를 사용하는 것이 일반적이었지만, 그 경향이 다음과 같이 변화하고 있다.
- `PC → 개인 휴대용 기기`(`PMD: personal mobile device`): 배터리로 동작, 무선으로 인터넷에 연결, 터치스크린 또는 음성인식을 주로 사용
- `server → cloud computing`: 창고 규모의 컴퓨팅(`WSC: warehouse-scale computing`)으로 알려진 거대 규모의 데이터센터를 이용
<br>

### 컴퓨터의 성능: 메모리에서 에너지 효율성으로
1960~1970년대에 컴퓨터 성능을 제약하던 가장 큰 요소는 컴퓨터 메모리의 용량이었고, 프로그램의 성능을 향상시키려면 메모리 사용을 최소화하는 것이 중요했다. 하지만, 오늘날 컴퓨터 설계와 메모리 분야의 기술이 발전하면서 임베디드 기술을 제외한 대부분의 응용에서 메모리 사용을 최소화해야 할 필요성은 크게 줄어들었다. <br>
메모리의 계층성과 프로세서의 병렬성이라는 두 이슈는 1960년대의 단순한 메모리 모델을 사라지게 만들었다. <br> 따라서 오늘날의 프로그래머는 PMD나 cloud에서 수행되는 프로그램의 에너지 효율성을 신경써야 하며, 이를 위해서는 코드 아래의 계층에서 무슨 일이 일어나는지 잘 이해해야 한다.
<br>
<br>



## 1.2 컴퓨터 구조 분야의 8가지 위대한 아이디어
<br>

다음 아이디어들은 60년여의 컴퓨터 역사 가운데 컴퓨터 구조 분야에서 발명된 8개의 아이디어들이다.
1. `Moore's law를 고려한 설계`: 컴퓨터 하드웨어 성능이 아주 빠르게 변화하는 것에 발맞춰, 컴퓨터 설계자는 프로젝트의 시작 시점보다 종료 시점의 기술을 예상해야 한다.
> **`Moore's law`** <br>
> 18~24개월마다 칩에 집적되는 소자의 수가 2배가 된다는 법칙. Gordon Moore가 1965년에 예측하였다.
2. `추상화`: 하위 수준의 상세한 사항을 보이지 않게 함으로써 상위 수준의 모델을 단순화한다.
3. `자주 생기는 일을 빠르게`(`common case fast`): 드물게 생기는 일을 최적화하는 것보다 자주 생기는 일을 빠르게 만드는 것이 성능 개선에 도움이 된다.
4. `병렬성(parrallelism)을 통한 성능 개선`: 작업을 병렬적으로 수행하여 성능을 높이도록 설계할 수 있다.
5. `파이프라이닝(pipelining)을 통한 성능 개선`: 짧은 파이프들을 연결하는 것처럼, 특정한 역할을 수행하는 각 스테이지들을 만듦으로써 성능을 향상시킬 수 있다.
6. `예측을 통한 성능 개선`: 예측을 잘못해서 이를 복구하는 비용이 비싸지 않고 예측이 성공할 확률이 비교적 높을 경우, 예측을 해서 일을 수행하는 것이 평균적으로 빠른 경우가 종종 있다.
7. `메모리 계층 구조`: 메모리의 속도, 용량, 가격이라는 상충되는 요구를 해결한다.
8. `여유분을 이용한 신용도 개선`: primary server와 standby server를 이용해 이중화하는 것처럼, 여유분을 준비해 신용도를 개선한다.
<br>
<br>



## 1.3 프로그램 밑의 세계
<br>

### 프로그램 밑에서 이런 친구들이 돌아갑니다

| `구분` | `종류` |
|:---:|:---|
| `applications software` | internet browser, word processor |
| `systems software` | compiler, assembler, loader, operating system |
| `hardware` | CPU, RAM |
<br>

### 우리가 쓰는 코드는 이렇게 해석됩니다
> **`high level programming language`**
> → *`compiler`* 
> → **`assembly language`**
> → *`assembler`* 
> → **`binary digit`(`bit`)**

<br>
<br>



## 1.4 케이스를 열고
<br>

컴퓨터의 고전적 요소 5가지는 입력, 출력, 메모리, 데이터패스(datapath), 제어(control) 유닛이며, 이 중 마지막 2개는 processor라고 부르기도 한다.

### 입출력
- `디스플레이`: 각 화상은 `픽셀`의 행렬로 구성되며, 이것은 `비트맵`(`bitmap`)이라고 부르는 비트들의 행렬로 표현된다. 컬러 디스플레이는 RGB 각각마다 8비트씩, 모두 24비트를 사용하여 색상을 표시할 수 있다.
> **`프레임 버퍼`(`frame buffer, or raster refresh buffer`)** <br>
> 비트맵을 기억하는 메모리. 그리팩 하드웨어는 스크린에 표시될 화상을 이곳에 저장했다가, 재생 속도에 맞춰 그래픽 디스플레이를 보낸다.

- `네트워크 인터페이스`: 컴퓨터 네트워크를 통해 컴퓨터가 연결되게 해준다.
  
> **`LAN`(`local area network`)** <br>
> 지리적으로 제한된 지역에서 데이터를 주고받도록 설계된 네트워크
<br>

> **`WAN`(`wide area network`)** <br>
> 대륙 전체를 연결할 수 있는 수백 km 이상의 네트워크
- 마우스, 키보드, 게임패드, 웹캠, 프린터, 터치스크린, ...

### 프로세서
- `프로세서`: 프로그램의 지시대로 일을 하는 부분. 숫자를 더하고, 검사하고, IO device에 신호를 보내 작동을 지시하는 등의 일을 한다.
- 
> **`데이터패스`** <br>
> 프로세서의 근육에 해당한다. 연산을 수행하는 부분이다.
<br>

> **`제어 유닛`** <br>
> 프로세서의 두뇌에 해당한다. 명령어에 따라 데이터패스, 메모리, IO device가 할 일을 지시한다.
<br>

> **`명령어 집합 구조`(`ISA: instruction set architecture`)** <br>
> 하드웨어와 소프트웨어 계층 사이를 이어주는 인터페이스이다. 이를 통해 구조와 구현을 분리하여 생각할 수 있도록 해준다.

### 메모리
- 메모리: 데이터의 저장소이다.
> **`volatile memory`(=`main memory` =`primary memory`)** <br>
> e.g. DRAM
<br>

> **`nonvolatile memory`(=`secondary memory`)** <br>
> e.g. DVD, magnetic disk, flash memory
<br>
<br>



## 1.5 프로세서와 메모리 생산 기술
<br>

### 집적회로(IC)
- `트랜지스터`(`transistor`): 전기로 제어되는 온/오프 스위치.
- `IC`(`integrated circuit`): 많은 수의 트랜지스터를 칩 하나에 집적시킨 것.
- `실리콘 결정 괴`(`silicon crystal ingot`) → `웨이퍼(wafer)` → `다이`, `칩`(`die` =`chip`)
<br>
<br>



## 1.6 성능
<br>

### 성능의 정의
컴퓨터의 성능은 그 컴퓨터를 사용하는 사용자의 입장에 따라 여러가지로 정의될 수 있다. <br>
예를 들어 PC를 사용하는 개인의 입장에서는 `응답시간`(`response time` =`execution time`)이, 데이터센터 관리자에게는 `처리량`(`throughput` =`bandwidth`)가 중요하다.
<br>

### 성능의 측정
프로그램 실행시간은 프로그램을 처리하는 데 걸린 시간을 초 단위로 표시한 것으로, 시간을 재는 방법에 따라 여러가지로 정의할 수 있다.
- `응답시간`(`response time` =`wall-clock time` =`elapsed time`): 한 작업을 끝내는데 필요한 전체 시간. 디스크 접근, 메모리 접근, 입출력 작업, OS 오버헤드 등 모든 시간을 다 더한 것이다.
- `CPU 실행 시간`(`CPU execution time` =`CPU time`): 프로세서가 순수하게 이 프로그램을 실행하기 위해 소비한 시간. 입출력에 걸린 시간이나 다른 프로그램을 실행하는 데 걸린 시간은 포함되지 않는다. `사용자 CPU 시간`과 `시스템 CPU 시간`의 합이다.
    - `사용자 CPU 시간`(`user CPU time`): 실제로 사용자 프로그램 실행에 소요된 시간이다.
    - `시스템 CPU 시간`(`system CPU time`): 운영체제가 이 프로그램을 위한 작업을 수행한 시간이다.
    >  운영체제의 각 작업이 어떤 프로그램을 위해 수행되고 있는지 명확히 가려내는 것이 어렵고, 운영체제 간의 기능 차이도 있기 때문에, 사용자 CPU 시간과 시스템 CPU 시간을 정확하게 구하는 것은 쉽지 않다.
<br>

### 성능 인자
- `클럭 사이클`(clock cycle): 클럭의 시간 간격
- `클럭 주기`(clock period): 한 `클럭 사이클`에 걸리는 시간이다. `250ps`와 같이 표시한다.
- `클럭 속도`: `클럭 주기`의 역수이다. `4GHz`와 같이 표시한다.
> 프로그램의 `CPU 실행 시간` = `프로그램의 CPU 클럭 사이클 수` / `클럭 속도`
- `CPI`(clock cycles per instruction): 명령어 당 클럭 사이클 수이다. 
> `CPU 클럭 사이클 수` = `명령어 수` * `명령어 당 평균 클럭 사이클 수`
<br>

### 성능 식
응답시간을 중심으로 한 성능의 정량적 식: <br>
> <img src="https://latex.codecogs.com/gif.latex?performance&space;=&space;\frac{1}{execution&space;time}" title="performance = \frac{1}{execution time}" />
고전적인 CPU 성능식: <br>
> <img src="https://latex.codecogs.com/gif.latex?cpu&space;time&space;=&space;instructions&space;*&space;CPI&space;*&space;clockcycle&space;=&space;\frac{instructions&space;*&space;CPI}{clock&space;speed}" title="cpu time = instructions * CPI * clockcycle = \frac{instructions * CPI}{clock speed}" />
<br>
<br>



## 1.7 전력 장벽
<br>

### 전력 장벽이 이끌어낸 멀티프로세서 패러다임
프로세서의 클럭 속도와 소비 전력은 오랫동안 빠르게 증가하다가 2000년대 중반부터 주춤해졌다. <br> 성장이 정체된 이유는 상용 마이크로프로세서의 냉각 문제 때문에 실제로 사용할 수 있는 전력이 한계에 도달했기 때문이다. PMD에서는 배터리 수명을 늘리는 것이, 그리고 데이터센터 등 서버를 설계하는 사람들에게는 서버에 전력을 공급하고 냉각하는 비용을 줄이는 것이 중요한 문제이기 때문에 포스트 PC 시대에 에너지는 더욱 중요한 요소가 되었다. <br>
집적회로의 주된 기술인 `CMOS`(complementary metal oxide semiconductor)가 에너지를 소비하는 주 원인은 `동적 에너지`(`dynamic energy`, 트랜지스터가 0→1, 또는 1→0으로 스위칭하는 동안에 소비되는 에너지)이므로 클럭 속도를 계속해서 늘리는 것에는 한계가 있다. <br>
새로운 공정기술이 나올 때마다 전압을 낮춰 그에 따라 소비전력(P=IV^2)도 줄일 수 있었지만, 전압을 더 이상 낮추게 되면 트랜지스터가 꺼져 있을 때에도 흐르는 누설전류가 너무 커진다는 문제가 있다. 서버 칩에서 현재 이미 40%의 전력이 누설에 의해 소모되고 있을 정도이다. 칩을 냉각시키는 더 비싼 방법을 사용할 수도 있지만, PMD, PC, 그리고 server에서조차도, 냉각을 위해 사용되는 부피가 너무 크거나, 그 비용이 감당하기 힘들 정도로 비싸다. <br>
따라서, 위에서 설명한 '전력 장벽'을 이기고 앞으로 나아가기 위해, 컴퓨터 설계자들은 지난날 마이크로프로세서를 설계했던 것과는 다른 방식인 '멀티프로세서' 방식을 선택했다.
<br>



## 1.8 현저한 변화: 단일 프로세서에서 멀티프로세서로의 변화
<br>

### 용어 정리
- `프로세서`: 회사들이 '코어'라고 부르는 것.
- `마이크로프로세서`: 회사들이 '멀티코어 마이크로프로세서', 혹은 단순히 '프로세서'라고 부르는 것.
>  쿼드코어 마이크로프로세서에는 4개의 프로세서가 들어있다.
<br>

### 프로그래머들이 바라보는 멀티프로세서로의 변화
단일 프로세서에 비해 마이크로프로세서는 response time보다는 throughput 개선에 더 효과가 있다. <br> 과거 프로그래머들은 코드를 한 줄도 바꾸지 않고 하드웨어, 컴퓨터 구조, 컴파일러의 혁신에만 의존하여도 아주 큰 성능 개선을 누릴 수 있었지만, 오늘날에는 다중 프로세서의 장점을 살리도록, 새로운 마이크로프로세서에서 더 빨리 실행되도록, 코어의 수가 증가될 때마다 코드를 지속적으로 개선해야 한다. <br> 이때 병렬 프로그래밍이 프로그래밍의 어려움을 가중시킨다는 점, 되도록 비슷한 일을 동시에 수행하도록 일을 분할, 스케줄링, 조정해야 한다는 점, 통신 및 동기화 오버헤드를 줄여야 한다는 점 등이 걸림돌이 될 수 있다.
<br>
<br>



## 1.9 실례: Intel Core i7 벤치마킹
<br>

- `벤치마크`(benchmark): 성능을 측정하기 위해 선택된 프로그램의 집합
- `SPEC`(System Performance Evaluation Cooperative): 최신 컴퓨터 시스템을 위한 표준 벤치마크를 만든다. (e.g. `SPEC CPU2006`에서는 정수형 벤치마크 12개(`CINT2006`)과 부동소수점 벤치마크 17개(`CFP2006`)으로 이뤄져 있다.)
<br>
<br>



## 1.10 오류 및 함정
- *함정*: 컴퓨터의 한 부분만 개선하고 그 개선된 양에 비례해서 전체 성능이 좋아지리라 기대하는 것은 흔한 착각이다.
> `Amdahl's law` <br>
> `개선 후 실행시간` = (`개선에 의해 영향을 받는 실행시간` / `개선의 크기`) + `영향을 받지 않는 실행시간`
- *오류*: 이용률이 낮은 컴퓨터는 전력 소모가 작다. → 서버는 작업부하가 가변적이기 때문에 이용률이 낮을 때의 전력효율이 중요하다. 실제 예에서, 한 서버는 이용률이 대부분 10%~50%이고 이용률이 100%인 경우는 전체 시간의 1%도 되자 않는다고 한다. 하지만 작업부하가 겨우 10%일 때 그 서버는 최대 전력의 33%나 소비하고 있었다.
- *오류*: 성능에 초점을 둔 설계와 에너지 효율에 초점을 둔 설계는 서로 무관하다. → 실행시간이 짧아지면 시스템의 전체 에너지가 절약될 수 있다.
- *함정*: 성능식의 일부분을 성능의 척도로 사용하는 것은 흔한 실수이다. → `클럭 속도`, `명령어 개수`, `CPI` 중 1~2개만을 사용하여 성능을 비교하는 방법은 오용 가능성이 크다.
<br>
<br>



## 1.11 결론
<br>

- 하드웨어와 하위 소프트웨어 간의 인터페이스인 ISA를 고정시키면 동일한 소프트웨어를 실행시키면서도 가격과 성능이 서로 다른 여러 구현이 가능하다.
- 실제 실행시간을 척도로 사용하면 믿을 만한 성능 판정이 가능하다. 실행시간은 다음과 같이 다른 중요한 척도들과 연결되어 있다.
> `초`/`프로그램` = `명령어 수`/`프로그램` * `클럭 사이클 수`/`명령어` * `초`/`클럭 사이클`
- 컴퓨터 기술 발전 속도에 대한 이해가 필요하다. 실리콘 반도체가 하드웨어 발전의 원동력이 되는 한편, 프로그램이 가진 병렬성을 이용하거나(e.g. 멀티프로세서) 메모리 계층구조에 대한 접근 지역성(locality)를 이용하는 등(e.g. cache memory) 컴퓨터 구조상의 새로운 아이디어는 가격과 성능의 비, 즉 가성비를 개선하는 역할을 한다.
- 마이크로프로세서 설계에서 가장 중요한 자원이 이전의 다이 면적에서 에너지 효율로 바뀌었다. 하드웨어는 멀티코어 마이크로세서로 방향을 전환했고, 이에 따라 소프트웨어도 병렬 하드웨어를 프로그래밍하는 쪽으로 방향을 전환하고 있다.
<br>
<br>

------
