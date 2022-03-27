# 프로토타입 패턴(prototype pattern) 

## 정의

특정 클래스의 인스턴스를 만드는 것이 자원/시간을 많이 잡아먹거나 복잡한 경우에 사용하는 패턴

### 구현 방법

간단한 구현 방법으로는 기존 인스턴스를 복제하여 새로운 인스턴스를 만드는 것이다.

따라서 아래 예시코드와 같이 간단하게 Cloneable을 구현해서 clone() 메서드를 재정의하여 구현할 수 있다.

```java
public class Prototype implements Cloneable {

    ...

    @Override
    public Prototype clone() {
	    try {
	        return (Prototype) super.clone();
    	} catch (CloneNotSupportedException e) {
	        throw new AssertionError();
	    }
    }
}
```

### 프로토타입 패턴의 장점
    - 복잡한 객체를 만드는 과정을 몰라도 사용할 수 있다.
    - 기존 객체를 복제하는 과정이 새 인스턴스를 만드는 것보다 비용(시간 또는 메모리)적인면에서 효율적일 수 있다.
    - 추상적인 타입을 리턴할 수 있다.

### 프로토타입 패턴의 단점
    - 복제한 객체를 만드는 과정 자체가 복잡할 수 있다.(특히, 순환 참조는 더욱)

### 프로토타입 패턴 활용
    - 자바 Object 클래스의 clone 메서드와 Cloneable 인터페이스
    - shallow copy와 deep copy
    - ModelMapper

### 참고

- 인프런 강의 - 코딩으로 학습하는 GoF의 디자인 패턴(백기선)
- Head First Design Patterns - Chapter 14. 부록 기타 패턴 - 프로토타입 패턴


