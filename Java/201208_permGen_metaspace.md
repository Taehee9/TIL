# permGen과 Metaspace

## permGen
permanent Generation

Java 7 에서 heap 메모리 영역 중 하나이다.  
static 객체와 String 상수, 그리고 클래스와 메소드의 메타데이터를 저장하는 곳이다.  
JVM에 의해 크기가 강제되는 영역

## Metaspace
Java 8 에서 native 메모리 영역 중 하나다.  
기존 permGen에서 저장하는 것들 중   
   * static object와 String 상수를 Heap으로 이동  
        → 최대한 GC의 대상이 될 수 있도록  
   * 나머지 정보들(메타 데이터)을 저장  

OS에서 메모리 관리하므로 메모리 부족시 자동으로 늘려준다.

### **옵션**
* -XX:MetaspaceSize  
   기본으로 제공되는 것보다 더 많은 메모리 사용할 것이라고 확신할 경우에 네이티브 메모리양을 변경하는데 사용되는 옵션
* -XX:MaxMetaspaceSize  
    최대 metaspace 영역의 크기 제한

## permGen → Metaspace 장점

- heap 영역에서 사용할 수 있는 메모리 증가
- permGen 스캔하기 위해 소모되었던 시간 감소  
    → GC 성능 향상

[JDK8에선 PermGen이 완전히 사라지고 Metaspace가 이를 대신 함.](https://starplatina.tistory.com/entry/JDK8%EC%97%90%EC%84%A0-PermGen%EC%9D%B4-%EC%99%84%EC%A0%84%ED%9E%88-%EC%82%AC%EB%9D%BC%EC%A7%80%EA%B3%A0-Metaspace%EA%B0%80-%EC%9D%B4%EB%A5%BC-%EB%8C%80%EC%8B%A0-%ED%95%A8)
