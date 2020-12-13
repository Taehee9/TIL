# default Method
Java 8 이 등장하면서 Interface에 대한 정의가 몇 가지 변경되었다.

## default Method 란?
인터페이스에 메소드를 구현하는 것

- 자바 API의 호환성을 유지하면서 라이브러리 변경 가능 → 유연해짐
- default를 쓰고 접근 제어자는 public이며 생략 가능하다.
- implements한 클래스에서 재정의가 가능

### default method를 쓰는 이유는?
인터페이스가 변경이 되면 인터페이스를 구현하는 모든 클래스들이 해당 메소드를 구현해야 하는 문제가 있다.  
이 문제를 해결하기 위해 인터페이스에 메소드를 구현해 놓을 수 있도록 하였다.

### default method로 추가된 2가지 메소드
1. **List 인터페이스의 sort 메소드**

    ```java
    default void sort(Comparator<? super E> c) {
    	Collections.sort(this, c);
    }
    ```

2. **Collection 인터페이스의 stream 메소드**

    ```java
    default Stream<E> stream() {
      return StreamSupport.stream(spliterator(), false);
    }
    ```

## default method 충돌시
1. 여러 인터페이스의 default method 간 충돌  
    인터페이스를 구현한 클래스에서 default method를 오버라이딩 해야 한다.

2. default method와 조상 클래스의 메소드 간 충돌  
    조상 클래스의 메소드가 상속되고 default 메소드는 무시된다.

## 추상 클래스와 인터페이스의 차이
default method 가 생기면서 추상 클래스와 인터페이스의 같은 점이 많아졌다.

### 다른점
- 인터페이스는 private 값을 가지지 못한다
- 인터페이스는 변수를 가질 수 없다.
- 추상 클래스는 하나만 상속 가능, 인터페이스는  여러개 구현 가능  
    → default 메소드 포함하는데 여러 개 구현  
    → 다중 상속 가능
- 인터페이스는 생성자를 가질 수 없다.

# static method 
static method 안에서 인스턴스 변수 접근이 불가능하다. (static 변수는 접근 가능)  
보통 Utility 성 메소드 작성 시 사용한다.

ex - 오늘 날짜 구하기 , 숫자에 콤마 추가하기

## 참고 
[Java의 정석](http://www.yes24.com/Product/Goods/24259565)  
[[Java8] 2. Default Method(디폴트 메서드)](https://asfirstalways.tistory.com/353)  
[Java8 인터페이스 default Method (디폴트 메소드)](https://wedul.site/320)  
[위키독스](https://wikidocs.net/228)
