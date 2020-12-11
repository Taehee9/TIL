혹시라도 프로세스와 쓰레드에 대한 내용을 까먹었다면 아래 글을 참조  
[Taehee9/TIL/프로세스와 쓰레드](https://github.com/Taehee9/TIL/blob/main/OS/201203_process_and_thread.md)

# 멀티 프로세스 
프로세스가 여러 개 있는 것  
프로세스 간의 통신을 하려면 IPC를 통해야 함  
- 프로세스 context switching 시 CPU 레지스터 교체 뿐만 아니라 RAM, CPU 사이의 캐시메모리에 대한 데이터 초기화

### IPC 란?  
Inter Process Communication  
프로세스 간 통신을 진행할 때 사용 하는 것

# 멀티 쓰레드  
하나의 프로세스가 다수의 쓰레드 동시에 작동 가능  
code, data, heap 영역은 공유

## 장점 
- 메모리 공간 줄어듬
- 시스템 자원 소모 줄어듬
- 공유 공간이 있기 때문에 데이터 주고받기 가능  
    프로세스보다 통신 방법이 간단
- context switching 시 공유 메모리만큼의 시간(자원) 손실 줄어듬  
    프로세스랑 달리 캐시 메모리 안비움

## 문제점
데이터와 힙 영역을 공유함으로 인해 문제가 생긴다.  
멀티 쓰레드 환경에서는 동기화 작업이 필요하다.  
이로 인해 병목현상이 발생해 성능이 저하될 가능성이 높다.  
그러므로 과도한 락으로 인한 병목현상을 줄여야 한다.

# 멀티쓰레드 vs 멀티 프로세스
대상 시스템 특징에 따라 적합한 동작 방식을 선택하고 적용해야 한다.  

**멀티 쓰레드**는 적은 메모리 공간, context switching이 빠르다.   
하지만, 오류로 인해 하나의 쓰레드가 종료되면 전체 쓰레드가 종료될 수 있고, 동기화 문제도 있다.

**멀티 프로세스**는 하나의 프로세스가 죽더라도 다른 프로세스에 영향을 끼치지 않다.  
하지만 멀티 쓰레드보다 많은 메모리 공간, CPU 시간을 차지한다. 

그러므로 대상 시스템에 따라 적합한 동작 방식을 선택하자!


# 쓰레드 동기화
Thread Synchronization  
공유데이터에 대해 쓰레드들의 동시 접근을 방지하는 해결책

## Java 쓰레드 동기화를 위한 synchronized 키워드
synchronized 로 감싸고 있는 부분에는 **하나의 쓰레드**만이 들어갈 수 있다.  
특정 쓰레드가 공유 데이터에 접근 했다면 다른 쓰레드가 데이터에 접근하더라도 먼저 들어간 쓰레드가 나올 때까지 **대기(wait)**해야 한다.

### 사용 방법
① 임계영역을 중괄호( { } )를 이용하여 지정  
```java
void execute() {
	...
	synchonized(this) {
		int sumResult = getCurrentSum();
		sumResult += 10;
		setCurrentSum(sumResult);
	}
	...
}
```

② 메서드 전체를 임계 영역으로 지정  
```java
synchronized void add() { 
	int sumResult = getCurrentSum();
	sumResult += 10;
	setCurrentSum(sumResult);
}
```

위 2가지 방법을 이용하면 해당 블록 안에는 하나의 쓰레드만이 블록에 있을 수 있다.

### 일정한 순서가 있는 경우
위의 예시가 아니라 비디오를 재생하는 경우를 생각해보자.  
영상은 버퍼링이 걸려 아직 데이터를 받기 전인데 음악 재생쓰레드가 재생을 해버린다면 화면은 뜨지 않고 음악만 나올 수 있다.  

이렇게 쓰레드가 일정한 순서가 있어야 하는 경우에는 wait(), notify() 메서드를 사용해야된다.

**wait()와 notify() 메서드**  
이 메서드들은 필수적으로 synchronized 임계영역 안에 있어야 한다.

- wait()  
    다른 쓰레드가 이 객체의 notify()를 불러줄 때까지 대기
- notify()  
    대기 중인 쓰레드를 깨운다.   
    2개 이상일 경우에는 랜덤으로 하나의 쓰레드를 깨운다.
- notifyAll()  
    대기 중인 모든 쓰레드를 깨운다.  
    한 번에 깨우는 것이 아니라 임의로 배정된 순서대로 깨운다.

```java
class SharedArea{
   double result;
   boolean isReady;
}

public class Ex {
   public static void main(String[] args) {
      CalcThread thread1 = new CalcThread();
      PrintThread thread2 = new PrintThread();
      SharedArea obj = new SharedArea();

      thread1.sharedArea = obj; 
      thread2.sharedArea = obj; 

      thread1.start();
      thread2.start();
      thread2.join();
   }
}

class CalcThread extends Thread{
   SharedArea sharedArea; 
   public void run(){
      double total = 0.0;

      for(int cnt=1;cnt<100000000;cnt+=2){
         if(cnt / 2 % 2 ==0)
            total += 1.0 / cnt;
         else
            total -= 1.0 / cnt;
      }

      sharedArea.result = total + 4;
      sharedArea.isReady=true;

      synchronized(sharedArea){
         sharedArea.notify();
      }
   }
}

class PrintThread extends Thread{
   SharedArea sharedArea;

   public void run(){
      if(sharedArea.isReady != true){
         try{
            synchronized(sharedArea){
               sharedArea.wait();
            }
         } catch(InterruptedException e){
            System.out.println(e.getMessage());
         }
      }
      System.out.println(sharedArea.result);
   }
}
```

참고 블로그 - 
[스레드 동기화 (Thread Synchronization) [Java]](https://m.blog.naver.com/sooftware/221424562029)

- SharedArea의 isReady 필드의 역할은?
    ```java
    if(shareArea.isReady != true) 
    ```  
    위의 코드가 없는 경우에 thread2에서 버퍼링이 걸려서 thread1이 모든 계산을 끝내고 notify() 를 실행한 뒤에 thread2가 실행되어 wait()가 실행된다면 프로그램은 **무한대기** 상태이기 때문이다.

- 왜 wait() 과 notify() 는 항상 synchronized 안에 있어야 하는가?  
    wait()을 블록 안에 안넣었으면 wait() → notify()의 순서가 꼬일 수 있다.  
    이 순서를 항상 성립하게 하기 위해 동기화 블록 안에 넣은 것이다.  
    
- 코드의 쓰레드는 몇 개이며 종료 가능한 순서와 이유는?  
    쓰레드는 main 쓰레드, thread1, thread2, garbage collector로 총 4개이다.   
    thread1 → thread2 → main 쓰레드 순으로 종료된다.  
    - garbage collector는 기본적으로 프로그램 실행 시 실행되는 쓰레드이다.


** join() 메서드는 해당 쓰레드 종료까지 자신도 종료하지 않고 대기하는 메서드

## 참고
[멀티 프로세스(Multi Process)와 멀티 스레드(Multi Thread)](https://you9010.tistory.com/136)  
[스레드 동기화 (Thread Synchronization) [Java]](https://m.blog.naver.com/sooftware/221424562029)
