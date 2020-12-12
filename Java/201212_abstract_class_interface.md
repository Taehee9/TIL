# 추상 클래스와 인터페이스
## 추상 클래스
추상 메소드가 하나 이상 포함되거나 abstract로 정의된 경우  
추상 메소드를 포함하고 있다는 것 제외하고는 일반 클래스와 전혀 다르지 않다.  
생성자가 있고 멤버 변수와 메서드도 가질 수 있다.

### 추상 메소드와 빈 몸통의 일반 메소드의 차이
abstract를 붙여서 추상 메소드로 선언해놓으면 자손 클래스에서 추상 케소드를 반드시 구현하도록 강요할 수 있기 때문에

### 예시
```java
abstract class Player {
	boolean pause;
	...
	abstract void play(); // 추상 메소드
	void play() {
		play(currentPos);
	}
}
```

## 인터페이스
추상 클래스처럼 추상 메소드를 갖지만 일반 메소드 또는 멤버 변수를 가질 수 없다.  
추상메소드와 상수만을 멤버로 가질 수 있다.  
다중 상속이 가능하다.

### 제약 사항  
- 모든 멤버 변수는 public static final 이어야 하며 이를 생략 가능하다.
- 모든 메소드는 public abstract 여야하며 이를 생략할 수 있다.
- static 메소드와 default 메소드는 예외(Java 8부터)

### 예시

```java
interface InterfaceName {
	public static final String name = value;
	public abstract method(param);
}
```

##  Java8부터 Interface
### default method & static method
원래는 인터페이스에 추상 메서드만 선언할 수 있는데, Java 8부터 default 메소드랑 static 메소드가 가능하다.  

자세한 내용은 따로 빼서 정리해봐야겠다.

## 참고
[Java의 정석](http://www.yes24.com/Product/Goods/24259565)  
[자바의 추상 클래스와 인터페이스](https://brunch.co.kr/@kd4/6)
