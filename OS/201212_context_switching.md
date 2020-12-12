# Context Switching
## Context
CPU가 다루는 Task(Process / Thread)에 대한 정보로 대부분의 정보는 레지스터에 저장되며 PCB로 관리된다.

## Context Switching
CPU가 실행하는 프로세스가 교체되며 CPU에서 관리하는 Context가 교체되는 것을 말한다.

### Context Switching 필요한 이유는?
해당 Task 끝날 때 까지 다음 Task 가 기다려야 하기 때문에   
빠른 속도로 Task 를 바꿔가며 실행하기 때문에 사람의 눈으로는 실시간으로 돌아가는 것처럼 보인다.

### 과정
CPU는 PCB에 실행 중인 Context를 읽어와 내부에 있는 레지스터에서 관리한다.  
인터럽트가 발생하면 해당 프로세스 실행을 중지한다.  
다음 프로세스가 실행되기 위해 레지스터에서 종료될 프로세스의 Context를 PCB에 업데이트 한다.  
다음 프로세스의 Context를 PCB에서 메인 메모리로 올려 CPU가 실행할 수 있게 된다.

**Interrupt**  
여기서 인터럽트란 입출력 요청, CPU 사용 기간 만료, 자식 프로세스 생성 등이다.

**PCB(Process Control Block)**  
OS의 스케줄러에 의해 Context Switching 되는 프로세스의 정보 단위

**TCB(Thread Control Block)**  
쓰레드 라이브러리에 의해 Context Switching 되는 쓰레드의 정보 단위 

### 단점  
Context Switching 하는 동안 프로세스 실행될 수 없으므로, 잦은 컨텍스트 스위칭은 오버헤드를 발생시켜 CPU 성능을 저하시킬 수 있다.

## 참고
[[운영체제] Context Switching](https://velog.io/@sohi_5/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-Context-Switching-suaduxev)  
[Steps in Context Switching](https://stackoverflow.com/questions/7439608/steps-in-context-switching/7443719)  
[Context Switching이란?](https://nesoy.github.io/articles/2018-11/Context-Switching)  
[프로세스 스레드의 차이](https://www.crocus.co.kr/1403)
