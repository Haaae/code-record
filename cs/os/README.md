[프로세스, 스레드 관련 참고 블로그](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%E2%9A%94%EF%B8%8F-%EC%93%B0%EB%A0%88%EB%93%9C-%EC%B0%A8%EC%9D%B4#%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98_%EA%B0%9C%EB%85%90)

<details>
<summary>프로세스와 스레드</summary>
<div markdown="1">

[참고](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%E2%9A%94%EF%B8%8F-%EC%93%B0%EB%A0%88%EB%93%9C-%EC%B0%A8%EC%9D%B4#%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98_%EA%B0%9C%EB%85%90)

### 프로세스
    프로그램을 실행하면 운영체제로부터 필요한 메모리를 할당받아 인스턴스를 생성하게 되는데, 이것을 프로세스라고 한다. 
    
    다시 말하면 프로세스는 실행 중인 하나의 애플리케이션이다.
    
    이때 필요한 메모리 영역은 프로그램의 코드를 저장하는 Text, 전역 정적 변수들을 저장하는 Data, 지역 변수들을 저장하는 Stack, 동적으로 메모리를 할당하는 Heap이 있다.

---
### 스레드
    스레드는 하나의 프로세스 안에서 실행되는 여러 코드 흐름을 의미한다.
    
    프로세스가 갖고 있는 Text 영역, Data 영역, Heap 영역을 공유하지만, 스레드 내부에서 Stack을 별도로 할당받는다. 
    
    어떤 프로그램이든 하나의 주요 흐름을 실행하는 Main 스레드는 가지고 있다.
    
    Data 영역과 Heap 영역을 공유하기에 다른 스레드와 데이터를 통신할 수 있지만, 어느 스레드가 먼저 실행할 지 모르기에 동기화와 교착 상태에 각별한 주의가 필요하다.

---
### 프로세스와 스레드, 그리고 메모리

- Process(작업)는 최소 1개의 Thread(연산 흐름, 실질적 연산 주체)를 갖는다.
- OS는 Virtual Memory를 Process에게 할당한다.
- 이때 Process에 속한 모든 Thread는 Process의 Virtual Memory로 공간이 제약된다.
    - Thread Local Storage: 쓰레드마다 Stack 구조로 관리되는 메모리 공간
    - Heap: Process 내에서 공유되는 메모리 공간

</div>
</details>


<details>
<summary>프로세스와 스레드의 생명주기</summary>
<div markdown="1">

[참고](https://inpa.tistory.com/entry/%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%E2%9A%94%EF%B8%8F-%EC%93%B0%EB%A0%88%EB%93%9C-%EC%B0%A8%EC%9D%B4#%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98_%EA%B0%9C%EB%85%90)

추가 예정

</div>
</details>


<details>
<summary>멀티 프로세스와 멀티 쓰레드의 특징과 차이</summary>
<div markdown="1">

## 멀티 프로세스

운영체제에서 각 프로세스들은 동시에 실행할 수 있도록 멀티 프로세스로 동작한다. 이때의 멀티 프로세스는 엄밀한 의미에서 동시에 실행되는 것이 아니고, CPU가 연산하는 대상(프로세스)가, 혹은 CPU를 사용하는 프로세스 주체가 아주 빠른 속도로 바뀌면서 마치 동시에 실행되는 것처럼 보이는 것이다. 
        
즉, CPU 자원을 잘게 쪼갠 시간 내에 점유한다는 뜻이다.

하나의 프로세스가 CPU 자원을 새로이 점유하기 위해서는 해당 프로세스의 현재 상태를 불러와야 하며, 현재 cpu를 점유하는 프로세스의 상태를 저장해두어야한다. 이처럼 CPU에서 실행할 프로세스를 교체하는 기술을 컨텍스트 스위칭(Context Switching)이라고 한다.
    
현재 프로세스의 PC와 SP 등 정보를 저장하는 아주 작은 공간을 Process Control Block(PCB)라고 하며, 운영체제 영역에서 관리된다. PCB에는 Process ID(PID), 레지스터(PC, SP 등)들을 포함해 프로세스가 실행 중인 상태 정보를 저장한다.


</div>
</details>


<details>
<summary>컨텍스트 스위칭</summary>
<div markdown="1">

[참고](https://beststar-1.tistory.com/26#%EC%BD%98%ED%85%8D%EC%8A%A4%ED%8A%B8_%EC%8A%A4%EC%9C%84%EC%B9%AD(Context_Switching))

---
### 개요

`CPU(Control Processing Unit)`가 어떤 `프로세스(Process)`를 실행하고 있는 상태에서
운영체제(Operating System)의 `스케줄러(Scheduler)`가 `인터럽트(Interrupt)`를 진행하여 더 높은 우선순위를 가진 프로세스가 실행되어야 할 때,
스케줄러가 `레지스터(Register)`에 저장된 기존 프로세스 정보 값이나 상태 값을 `커널(Kernal) 내부에 존재하는 PCB(Process Control Block)`에 저장하고, `새 프로세스의 정보 값이나 상태 값을 PCB에서 다시 가져와 교체하는 작업`을 말한다.

한마디로 스케줄러가 기존 실행 프로세스를 우선순위 때문에 미루고 새 프로세스로 교체해야 할 때 프로세스 상태 값을 교체하는 작업을 말한다.

---

### 세부 내용

#### 원리

    콘텍스트 스위칭에서 콘텍스트란 CPU가 해당 프로세스를 실행하기 위한 프로세스의 정보를 말한다.

    콘텍스트 스위칭이 가능케 하는 핵심은 `프로그램 카운터(Program Counter, PC)`와 `스택 포인터(Stack Pointer, SP)`이다.

    - 프로그램 카운터(Program Counter, PC)

    CPU 내부에 있는 `다음에 실행할 명령어, 즉 기계어 코드의 위치를 가리키는 레지스터`이다. 간단히 코드 한줄한줄을 가리키는 주소 레지스터라고 생각하면 된다.

    - 스택 포인터(Stack Pointer, SP)

        CPU안에서 `스택에 데이터가 채워진 마지막 위치를 가리키는 레지스터`다. 스택 특성상 스택 포인터가 가리키는 곳까지가 데이터가 채워져 있는 영역이고 그 이후부터 스택의 끝까지는 데이터가 없는 영역인 것이다. 즉, 스택에 새로운 데이터가 추가되거나 제거되면 스택 포인터의 값이 증가하거나 감소한다.


#### 발생 시점

    콘텍스트 스위칭은 인터럽트 발생 시 실행된다.

    인터럽트란, CPU가 프로그램을 실행하고 있을 때 실행 중인 프로그램 밖에서 예외 상황이 발생하여 처리가 필요한 경우 CPU에게 알려 작동이 중단되지 않고 예외 상황을 처리할 수 있도록 하는 기능을 말한다.

    콘텍스트 스위칭은 아래와 같은 인터럽트 요청이 와야 발생한다.

    1. 입/출력을 요청할 때

    2. CPU 사용시간이 만료되었을 때

    3. 자식 프로세스를 만들 때

    4. 인터럽트 처리를 기다릴 때

    이런 요청들이 어떤 동작 속에서 들어오는지 이해하기 위해서는 프로세스의 상태변화를 알아야 한다.


#### 단점 

##### 오버헤드(Overhead)로 인한 성능 저하
    콘텍스트 스위칭이 잦게 발생할수록 오버헤드(Overhead) 비용이 발생하여 성능이 떨어진다. 여기서 오버헤드란 콘텍스트 스위칭에 사용된 시간과 사용된 메모리의 양을 말한다.

![context-switching-img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc3wWnK%2Fbtq2NcrH2wm%2FYikhwPpGK6p8Yu72QmDxi1%2Fimg.jpg)

    위 그림을 보면 프로세스 P0가 실행 중인 상태(excuting)에서 유휴 상태(idle)가 될 때 프로세스 P1이 곧바로 excuting이 되지 않고 idle을 좀 더 하다가 excuting이 된다.

    그 이유는 프로세스 P0의 상태를 PCB에 저장하고 프로세스 P1 상태를 PCB에서 가져와야 하기 때문이다.

    그런데, 이 과정에서 PCB를 저장하고 가져올 때 CPU는 아무런 일도 하지 못하게 된다.

    따라서 이 아무런 일도 하지 못하게 되는 상황이 잦을수록 당연히 성능 저하로 이어지는 것이다.

#### 스레드와 프로세스
![thread-img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUCpIg%2Fbtq2SLGLqyK%2Fh0n56LXdtdA2bl6mqJiAoK%2Fimg.png)

    스레드 단위로도 콘텍스트 스위칭이 가능하다.

    스레드는 콘텍스트 스위칭될 때 스택(Stack)을 제외한 코드(Code), 데이터(Data), 힙(Heap) 영역은 프로세스의 것이기 때문에 자신의 PCB에는 스택 및 간단한 정보만 저장해서 프로세스 콘텍스트 스위칭보다 빠르다.

    반면에 프로세스는 공유하는 데이터가 없으므로 임시 저장소인 캐시 메모리가 지금껏 쌓아놓은 데이터들이 사라지고 새로 데이터를 쌓아야 한다.

    따라서 콘텍스트 스위칭의 오버헤드를 개선하기 위해 스레드 컨텍스트 스위칭을 사용하기도 한다.

---
### 용어

- 스케줄러(Scheduler)
    
      어떤 프로세스에게 자원을 할당할지 순서와 방법을 결정하는 운영체제 커널의 모듈을 지칭한다.

- 레지스터(Register)

      n-bit의 정보를 저장할 수 있는 고속도의 기억장치를 말한다.

      CPU에서는 외부 요청을 처리하는 데 필요한 데이터를 일시적으로 저장하고 이동하는 고속도의 기억장치로 사용된다.

- 커널(Kernal)

      운영체제 중 항상 필요한 부분만을 전원이 켜짐과 동시에 메모리에 올려놓고 그렇지 않은 부분은 필요할 때 메모리에 올려서 사용하게 되는데, 이때 `메모리에 상주하는 운영체제의 부분`을 커널이라 한다.

      운영체제도 소프트웨어라서 메모리에 올라가야 하나 규모가 큰 프로그램이기 때문에 모두 올라간다면 한정된 메모리 공간의 낭비가 심할 것이기 때문에 커널로 분리해둔 것이다.

- PCB(Process Control Block)

      특정한 프로세스를 관리할 필요가 있는 정보를 포함하는 운영 체제 커널의 자료 구조를 말한다.

      그 정보에는 프로세스 번호, 포인터, 프로세스 상태, 레지스터, 프로그램 카운터(코드 위치) 등이 있다.

      번외로 스레드 단위의 정보를 저장하는 자료구조는 TCB라고 한다. 

---

</div>
</details>


<details>
<summary>PCB와 TCB</summary>
<div markdown="1">

## PCB

PCB는 운영체제가 프로세스를 제어하기 위해 정보를 저장해 놓는 곳으로, 프로세스의 상태 정보를 저장하는 CS 구조체이다.

프로세스의 상태 관리와 문맥 교환(Context Switching)에서 사용된다.

프로세스가 생성될 때마다 고유의 PCB 가 생성되며, 프로세스가 완료되면 PCB는 제거된다.

### PCB에 저장된 요소

![PCB](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FVXBmD%2FbtrtADpaHoj%2FvDizu05LeysOIGF06Sjd1K%2Fimg.png)

1. Process ID : 프로세스의 고유 ID
1. Process State : 프로세스의 상태 (Create, Ready, Running 등등)
1. Program Counter : 프로세스를 위해 실행될 다음 명령어의 주소
1. Register : Accumulator, General Register 등을 포함하는 CPU Register의 값
1. CPU Scheduling Information : 우선순위, 최종 실행시간, CPU 점유시간 등
1. Memory management information : 해당 프로세스의 주소 공간 정보
1. I/O Status : 프로세스에 할당된 입출력 장치 목록, 열린 파일 목록 등 

멀티스레드가 아닌 멀티프로세스 환경에서는 PCB가 PC와 Register Set 정보도 포함한다.

## TCB

TCB는 Thread별로 존재하는 자료구조이며, PC와 Register Set(CPU 정보), 그리고 PCB를 가리키는 포인터를 가진다.

![TCB](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsxO0J%2FbtqEwQ5PbRD%2FkrWKDTE60qcaJpksIFcAy1%2Fimg.jpg)

TCB는 해당 Thread에 대한 정보만 저장하면 되기 때문에 PCB보다 적은 데이터를 갖는다.

보통 TCB는 커널 레벨에서 Context Switching의 기본 단위가 되며, 같은 프로세스에서의 스위칭에 대해서는 TCB 정보만 저장하면 된다.

하지만 다른 프로세스 간의 스위칭을 할 때에는 PCB / TCB 정보를 모두 저장해야 한다.


</div>
</details>


<details>
<summary>동기와 비동기의 차이</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>멀티스레드 프로그래밍</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>Thread-safe 의 의미와 설계 방법</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>프로세스 동기화</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>교착 상태(DeadLock)와 기아상태 정의 및 해결방법</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>세마포어와 뮤텍스의 차이</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>Critical Section(임계영역)</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>가상 메모리란?</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>페이지 교체 알고리즘</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>캐시의 지역성이란?</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>CPU Scheduling과 콘보이 현상(convoy effect)</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>Shared Memory 방식과 Message Passing 방식의 차이</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>사용자 스레드와 커널 스레드의 차이</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>프로세스의 각 Section에는 무엇이 저장되는가?</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>프로세스 통신 방법의 종류</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>경쟁상태</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>경쟁상태가 발생하는 경우와 해결법</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>논리주소와 물리주소</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>메모리 할당 알고리즘</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>운영체제에서 페이징은 무엇인가?</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>페이징과 세그멘테이션 차이</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>TLB가 무엇인지, TLB miss와 hit가 일어나는 경우에 대해 설명해주세요.</summary>
<div markdown="1">

추가예정

</div>
</details>


<details>
<summary>프로세스 관련 용어</summary>
<div markdown="1">

PCB: 프로세스 제어 블록, 프로세스에 대한 중요한 정보를 저장합니다.

PC: 프로그램 카운터, 프로세스 실행을 위한 다음 명령의 주소를 표시합니다.

캐시메모리: 자주 사용되는 데이터가 저장되는 공간으로 CPU의 레지스터와 메모리 사이에서 병목 현상을 완화하는 장치입니다.

</div>
</details>