# 💥Operating System(Chapter 1 - Introduction)

---

> 본 내용은 WILEY 출판사에서 발간한 운영체제 10판(번역본)을 바탕으로 작성하였습니다.

운영체제란 기본적으로 컴퓨터 하드웨어를 관리하는 소프트웨어이다. 컴퓨터 사용자(User)와 컴퓨터 하드웨어(Hardware) 사이에 위치하며 중재자 역할을 수행한다.

![CSCI 6730 / 4730 Operating Systems Key Questions in System Design](../assets/img/os.png)

이 사진은 이러한 운영체제의 역할을 잘 보여준다. 운영체제는 그 자체로 유용한 기능을 수행하는 소프트웨어가 아니다. 운영체제는 단순히 다른 프로그램이 유용한 작업을 할 수 있는 환경을 제공하는 역할을 한다.

---

Modern computer system consists of one or more CPUs and a number of device controllers(ex. Disk Controller, USB Controller). Those are connected by common bus and they shared memory.

일반적으로 운영체제에는 각 장치 컨트롤러마다 장치 드라이버가 있다. 이 장치 드라이버는 장치 컨트롤러의 작동을 잘 알고 있고 나머지 운영체제에 장치에 대한 일관된 인터페이스를 제공한다.

---

### 운영체제의 목표는 크게 3가지로 분류할 수 있다.

1. Convenience - It is an interface between User & Hardware.
2. Efficiency -  Allocation of Resources.
3. Both - Management of Memory, Security, etc.

---

###  중요한 개념인 Bootstrap program, Interrupt, system call에 대한 간단한 설명

- #### Bootstrap Program

  > - The initial program that runs when a computer is powered up or rebooted.
  > - It is stored in the ROM(Read-Only-Memory).
  > - It must know how to load the OS and start executing that system.
  > - It must locate and load into Memory the OS kernel.

- #### Interrupt(주로 Hardware에서 발생, 굉장히 중요한 개념이기 때문에 아래에 설명을 추가하겠습니다.)

  > - The occurrence of an event is usually signaled by an Interrupt from Hardware or Software.
  >
  > - Hardware may trigger an interrupt at anytime by sending a signal to the CPU, usually by the way of the system bus.
  >
  > - Execution Progress 1 
  >
  >   When the CPU is interrupted, it stops what it is doing and immediately transfers execution to a fixed location.(The fixed location usually contains the starting address where the Service Routine of the interrupt is located.)
  >
  > - Execution Progress 2
  >
  >   The Interrupt Service Routine executes. On completion, the CPU resumes the interrupted computation.

- #### System Call(Software에서 발생)

  > - Software may trigger an interrupt by executing a special operation called System Call.

---

## 🔴Interrupts

> CPU가 프로그램을 실행하고 있을 때, 입출력 하드웨어 장치의 요청이나 예외상황이 발생하여 처리가 필요한 경우 CPU에게 알려서 처리할 수 있도록 하는 것

<br/>

하드웨어는 어느 순간이든 시스템 버스를 통해 CPU에 신호를 보내 인터럽트를 발생시킬 수 있다.

CPU가 인터럽트 되면

1. CPU는 하던 일을 중단하고 즉시 고정된 위치로 실행을 옮긴다.(고정된 위치란 일반적으로 인터럽트를 위한 서비스 루틴이 위치한 시작 주소이다.)
2. 인터럽트를 위한 서비스 루틴이 실행된다.
3. 인터럽트 서비스 루틴의 실행이 완료되면 CPU는 인터럽트 되었던 연산을 재개한다.

여기서 중요한 것은 인터럽트 되었던 연산을 재개하기 위해서는 해당 연산이 남아있어야 한다는 것이다. 하지만, 현재 CPU는 인터럽트를 위한 서비스 루틴을 실행한 직후이고 모든 값들이 인터럽트를 위한 서비스 루틴에 맞추어져 있는 상태이다.

때문에! 반드시 1번 과정 이전에 현재의 상태를 저장해두어야 한다. 상태를 저장해두어야 인터럽트를 위한 서비스 루틴을 수행한 후, 저장되어 있던 복귀 주소를 프로그램 카운터에 적재하고, 인터럽트에 의해 중단되었던 연산이 인터럽트가 발생하지 않았던 것처럼 다시 시작할 수 있다.(어떻게 어디에 저장하는지는 이후 챕터에서 나온다.)



![Exception and interrupt handling :: Operating systems 2018](../assets/img/exception-and-interrupt-handling-v1.png)

이 사진 한 장이 인터럽트의 과정을 잘 설명하고 있다. 하지만, 조금 더 구체적으로  인터럽트는 단순히 외부에서만 발생하지 않는다.

- 외부 인터럽트

> 하드웨어가 발생시키는 인터럽트
>
> CPU가 아닌 다른 하드웨어 장치가 CPU에 요청하는 경우
>
> 아마 외부 인터럽트가 일반 인터럽트의 대부분을 차지하지 않을까?

- 내부 인터럽트

  >CPU 내부에서 실행하면서 인터럽트에 걸리는 경우
  >
  >Divide by Zero, Overflow, Underflow

- 소프트웨어 인터럽트

  > 소프트웨어가 발생시키는 인터럽트
  >
  > 예외상황, system call

  <br/>

  `+` 인터럽트 기법에는 우선순위 레벨도 존재한다고 한다. 우선순위가 높은 인터럽트를 먼저 처리할 수도, 우선순위가 낮은 인터럽트를 늦게 처리할 수도 있다.

  <br/>

  <br/>

  **요약하면, 인터럽트는 최신 운영체제에서 비동기 이벤트를 처리하기 위해 사용된다. 장치 컨트롤러 및 하드웨어 오류로 인해 인터럽트가 발생한다. 가장 긴급한 작업을 먼저 수행하기 위해 최신 컴퓨터는 인터럽트 우선순위 시스템을 사용한다. 인터럽트는 시간에 민감한 처리에 빈번하게 사용되므로 시스템 성능을 좋게 하려면 효율적인 인터럽트 처리가 필요하다.**

---

## 🔴DMA(Direct Memory Access)

> 기본으로 알아야 하는 내용
>
> 1. OS 코드는 많은 부분들이 I/O를 manage 하는데 할당된다. 왜냐하면,  I/O의 reliability(신뢰성)와 performance(성능)이 중요하고, I/O 디바이스의 종류가 굉장히 다양하기 때문
> 2. Each device controller is in charge of a specific type of device. (Device controller maintains load storage buffer and set of special purpose registers.)
> 3. Typically OS has a device driver for each device controller.

<br/>

위에 언급한 interrupt를 통한 I/O는 소량의 데이터일 때는 괜찮지만, 대량의 데이터 이동 시에는 굉장히 높은 오버헤드를 발생시킬 수 있다.(당연하지 한 바이트마다 인터럽트가 발생할 것이고 인터럽트 자체가 pure overhead이다.)

이 문제를 해결하기 위해 DMA가 사용된다.  

DMA는 블록마다 딱 1번의 인터럽트가 발생한다.(디바이스 드라이버의 동작이 완료 되었을 때)

인터럽트를 거는 횟수가 월등히 적어지고 디바이스와 메모리가 직접적으로 데이터를 transfer하기 때문에 CPU는 그동안 다른 일을 처리할 수 있다.

![DMA](../assets/img/DMA.png)

---

![심심해서 하는 블로그 :: [컴퓨터 구조] 캐시 메모리(Cache)와 매핑 방법](../assets/img/mapping.png)

블록이란 메모리에서 캐쉬로 이동하는 데이터의 단위이다.

---

### 조금 더 심화된 의문. OS는 어떤 디바이스들이 연결되어 있는지 어떻게 알고 있는 것일까?

기본적으로 I/O  디바이스는 I/O 포트에 의해 머신에 연결된다. 디바이스는 자신만의 디바이스 컨트롤러를 갖는다. 디바이스 컨트롤러는 I/O machine과 OS, 그 중간에 위치한 하드웨어이다.

OS가 I/O 디바이스 조작을 위해 CPU에 Instructions를 보낸다. 그러면 이 CPU는 이 I/O 디바이스들에 시그널을 보낸다.(디바이스 컨트롤러를 통해) 그렇다면 OS는 어떤 디바이스들이 연결되어 있는지, 디바이스 working을 위해 어떤 instructions를 보내야 하는지 어떻게 알고 있는 것일까.

=> manufacture가 디바이스를 만들 때 `디바이스 프로그램` 또한 같이 배포하게 된다. 이 때문에 OS에는 디바이스에 대한 정보들이 전혀 존재하지 않는다. 따라서, OS가 디바이스와 상호작용할 수 있는 유일한 수단은 이것의 `디바이스 프로그램`을 통해서 이다. 많은 초심자들이 일단 드라이버가 설치만 되면 OS는 디바이스의 모든 정보를 이해한다고 생각한다. 하지만, 이것은 사실이 아니고, 드라이버는 OS의 한 부분이 절대 될 수 없다. 결국 드라이버 프로그램이 instructions을 보내고 디바이스로부터 데이터를 받으면 드라이버 프로그램이 그 정보를 OS에 전달하는 식의 플로우이다. OS와 드라이버 간의 소통은 굉장히 간단하게 이루어지는데 둘다 메모리에서 load 되는 것들이기 때문이다. 드라이버는 메모리 공간에서 오랫동안 존재하는 **daemon program**이라 생각할 수 있다. 이러한 이유들 때문에 드라이버 프로그램이 crash 되자마자 I/O 디바이스의 동작이 멈추는 것이다.

**위의 내용은 굉장히 심화된 내용이면 나 또한 부분부분 이해하지 못한 내용들이 많다. 언젠가 실력이 올라서 이 내용을 이해할 수 있는 순간이 오지 않을까라는 생각에 일단 적어둔다.**

---

##  Storage Structure

<br/>

![storage_structure](../assets/img/storage_structure.png)



- 캐쉬 - SRAM  메인 메모리 - DRAM  하드디스크 - HDD

- 범용 컴퓨터는 프로그램 대부분을 메인 메모리(randim-access-memory 또는 RAM 이라 부른다)라 불리는 재기록 가능한 메모리에서 가져온다. 메인 메모리는 dynamic random-access memory(DRAM)이라 불리는 반도체 기술로 구현한다.
- 캐쉬는 CPU 속에 들어가 있다.
- 보통 비휘발성 저장장치 전부를 `DISK`로 표현한다.(이건 정확하지 않을 수 있다. 근데 보통 그런 식으로 다들 그림을 그리는 것 같다.)

---

