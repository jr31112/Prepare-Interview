# 운영체제(OS)

## 스케줄링(Scheduling)

### 1. 스케줄링이란?

단일프로세서 시스템(Single Processor System)에서는 한 번에 한 프로세스만 실행될 수 있다. 하지만 프로세스가 항상 CPU를 사용하는 것은 아니다. 키보드나 마우스 등의 입력 장치에서 사용자의 입력을 기다리거나, 프린터 등의 느린 출력장치에서 데이터를 출력할 때 CPU는 일을 하지 않고 가만히 있는다. 여러 프로세스를 처리할 때, 한 프로세스가 모든 작업이 끝날때까지 기다렸다가 다음 프로세스를 실행하는 방법보다 **한 프로세스를 실행 가능한 시점까지 실행하고, I/O 등 CPU를 사용하지 않는 작업을 할 때는 다른 프로세스를 실행한다**면 CPU 사용 효율을 높일 수 있다. 그러므로 프로세스 스케줄링을 통해 프로세스가 실행되는것을 통제한다.=> 아주 빨리 교체해서 CPU코어가 모자름에도 병렬로 실행되는 것처럼 보인다. 

### 2. 프로세스 상태 및 스케줄링 큐

1. 프로세스 상태

 ![프로세스 상태](./process) 

* 생성(new) : 프로세스가 생성되는 중이다.
* 실행(running) : 프로세스가 프로세서를 차지하여 명령어들이 실행되고 있다.
* 준비(ready) : 프로세스가 프로세서를 사용하고 있지는 않지만 언제든지 사용할 수 있는 상태로, CPU가 할당되기를 기다리고 있다.
* 대기(waiting) : 프로세스가 입출력 완료, 시그널 수신 등 어떤 사건을 기다리고 있는 상태를 말한다.
* 종료(terminated) : 프로세스의 실행이 종료되었다.

2. 스케줄링 큐

  ![queueing diagram](./scheduling queue) 

- Job Queue : 현재 시스템 내에 있는 모든 프로세스의 집합
- Ready Queue : 현재 메모리 내에 있으면서 CPU 를 잡아서 실행되기를 기다리는 프로세스의 집합
- Device Queue : Device I/O 작업을 대기하고 있는 프로세스의 집합

새로운 프로세스는 처음에 ready queue에 놓인다. **프로세스는 CPU를 할당받을 때(dispatch)까지 ready queue에서 대기**한다. 프로세스에 CPU가 할당되어 실행되면 아래 사건들 중 하나가 발생할 수 있다.

* I/O(입출력)을 요청하여 I/O queue에 넣어질 수 있다.
* child process를 생성하고 그 프로세스의 종료를 기다릴 수 있다.
* interrupt의 결과에 따라 강제로 CPU로부터 제거되어 ready queue에 다시 놓일 수 있다.

프로세스는 결국 waiting state에서 ready state로 전환되고 다시 ready queue에 넣어진다. 프로세스는 종료될 때까지 이 주기를 계속하며, 종료되면 모든 queue에서 삭제되고 프로세스의 PCB와 자원을 반납(deallocate)한다.

### 스케줄러의 종류

![스케줄링1](./schedule1) 

 ![스케줄링2](./schedule2) 

1. 장기 스케줄러(Admission scheduler)

   하드 디스크에서 메모리로 프로세스를 load하는 역할을 한다. 즉 Secondary storage에 저장되어 있는 프로세스가 사용자에 의해 메모리에 적재되고 ready 상태로 되는 것이다. 

   - 메모리와 디스크 사이의 스케줄링을 담당.
   - 프로세스에 memory(및 각종 리소스)를 할당(admit)
   - degree of Multiprogramming 제어
     메모리에 여러 프로그램이 올라가는 것) 몇 개의 프로그램이 올라갈 것인지를 제어
   - 프로세스의 상태
     new -> ready(in memory)

   *cf) 메모리에 프로그램이 너무 많이 올라가도, 너무 적게 올라가도 성능이 좋지 않은 것이다. 참고로 time sharing system 에서는 장기 스케줄러가 없다. 그냥 곧바로 메모리에 올라가 ready 상태가 된다.*

2. 단기 스케줄러(CPU scheduler)

   ready queue에 존재하는 프로세스들 중에서 어떤 프로세스를 running state로 둘 지 선택한다. 

   - CPU 와 메모리 사이의 스케줄링을 담당.
   - Ready Queue 에 존재하는 프로세스 중 어떤 프로세스를 running 시킬지 결정.
   - 프로세스에 CPU 를 할당(scheduler dispatch)
   - 프로세스의 상태
     ready -> running -> waiting -> ready

3. 중기 스케줄러

   메모리에서 프로세스들을 제거하여, 즉 CPU를 위한 경쟁에서 제거하여, 다중 프로그래밍의 정도를 완화하는 것이다. 차후에 다시 프로세스를 메모리로 불러와서 중단되었던 지점에서부터 실행을 재개한다. 이러한 기법을 **swapping**이라고 한다. 

   - 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄 (swapping)
   - 프로세스에게서 memory 를 deallocate
   - degree of Multiprogramming 제어
   - 현 시스템에서 메모리에 너무 많은 프로그램이 동시에 올라가는 것을 조절하는 스케줄러.
   - 프로세스의 상태
     ready -> suspended

    Suspended(stopped) : 외부적인 이유로 프로세스의 수행이 정지된 상태로 메모리에서 내려간 상태를 의미한다. 프로세스 전부 디스크로 swap out 된다. blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스스로 ready state 로 돌아갈 수 있지만 이 상태는 외부적인 이유로 suspending 되었기 때문에 스스로 돌아갈 수 없다. 

### 스케줄링 기법

Ready Queue는 항상 FIFO구조가 아니다!!

1. 선점 / 비선점

2. 기법들
**FCFS(First Come First Served)**

* 먼저 온 고객을 먼저 서비스해주는 방식, 즉 먼저 온 순서대로 처리
  * 비선점형(Non-Preemptive) 스케줄링
  * 일단 CPU 를 잡으면 CPU burst 가 완료될 때까지 CPU 를 반환하지 않는다. 할당되었던 CPU 가 반환될 때만 스케줄링이 이루어진다.
  
  **문제점**
  
  * convoy effect : 소요시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상이 발생한다.
  
  
  
  **SJF(Shortest - Job - First)**
  
  * 프로세스가 먼저 도착했어도 CPU burst time 이 짧은 프로세스에게 선 할당
  * 비선점형(Non-Preemptive) 스케줄링
  
  **문제점**
  
  * starvation : 효율성을 추구하는게 가장 중요하지만 특정 프로세스가 지나치게 차별받으면 안되는 것이다. 이 스케줄링은 극단적으로 CPU 사용이 짧은 job 을 선호한다. 그래서 사용 시간이 긴 프로세스는 거의 영원히 CPU 를 할당받을 수 없다.
  
  
  
  **SRT(Shortest Remaining time First)**
  
  * 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.
  * 선점형 (Preemptive) 스케줄링
  * 현재 수행중인 프로세스의 남은 burst time 보다 더 짧은 CPU burst time 을 가지는 새로운 프로세스가 도착하면 CPU 를 뺏긴다.
  
  **문제점**
  
  * starvation : 새로운 프로세스가 도달할 때마다 스케줄링을 다시하기 때문에 CPU burst time(CPU 사용시간)을 측정할 수가 없다.
  
  
  
  **Priority Scheduling**
  
  * 우선순위가 가장 높은 프로세스에게 CPU 를 할당하겠다. 우선순위란 정수로 표현하게 되고 작은 숫자가 우선순위가 높다.
  * 선점형 스케줄링(Preemptive) 방식
  * 더 높은 우선순위의 프로세스가 도착하면 실행중인 프로세스를 멈추고 CPU 를 선점한다.
  * 비선점형 스케줄링(Non-Preemptive) 방식
  * 더 높은 우선순위의 프로세스가 도착하면 Ready Queue 의 Head 에 넣는다.
  
  **문제점**
  
  * starvation : 무기한 봉쇄(Indefinite blocking)
  
    실행 준비는 되어있으나 CPU 를 사용못하는 프로세스를 CPU 가 무기한 대기하는 상태
  
    - 해결책(aging) : 아무리 우선순위가 낮은 프로세스라도 오래 기다리면 우선순위를 높여주자.
  
  
  
  **Round Robin**
  
  * 현대적인 CPU 스케줄링
  * 각 프로세스는 동일한 크기의 할당 시간(time quantum)을 갖게 된다.
  * 할당 시간이 지나면 프로세스는 선점당하고 ready queue 의 제일 뒤에 가서 다시 줄을 선다.
  * `RR`은 CPU 사용시간이 랜덤한 프로세스들이 섞여있을 경우에 효율적`RR`이 가능한 이유는 프로세스의 context 를 save 할 수 있기 때문이다.
  
  **장점**
  
  * `Response time`이 빨라진다.
  * n 개의 프로세스가 ready queue 에 있고 할당시간이 q(time quantum)인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n 을 얻는다. 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.
  * 프로세스가 기다리는 시간이 CPU 를 사용할 만큼 증가한다. 공정한 스케줄링이라고 할 수 있다.
  
  **주의할 점**
  
  설정한 `time quantum`이 너무 커지면 `FCFS`와 같아진다. 또 너무 작아지면 스케줄링 알고리즘의 목적에는 이상적이지만 잦은 context switch 로 overhead 가 발생한다. 그렇기 때문에 적당한 `time quantum`을 설정하는 것이 중요하다.
