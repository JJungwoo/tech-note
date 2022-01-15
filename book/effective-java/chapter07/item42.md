# 익명 클래스보다는 람다를 사용하라

익명 클래스 방식은 코드가 너무 길기 때문에 자바는 함수형 프로그래밍에 적합하지 않았다.

```java
Collections.sort(words, new Comparator<String>() {
  public int compare(String s1, String s2) {
    return Integer.compare(s1.length(), s2.length());
  }
})
```

자바 8에서부터 함수형 인터페이스인 람다식(lambda expression, 람다)을 사용할 수 있다.<br>
람다는 함수나 익명 클래스와 개념은 비슷하지만 코드는 훨씬 간결하다.

```java
Collections.sort(words, 
     (s1, s2) -> Integer.compare(s1.length(), s2.length()));

// 비교자 생성 메서드 사용
Collections.sort(words, comparingInt(String::length));

// 자바8 List 인터페이스의 sort 메서드 사용
word.sort(comparingInt(String::length));
```

열거형 타입의 상수별 클래스 몸체에도 람다식을 적용할 수 있다.

```java
public enum Operation { 
    PLUS("+") { 
        public double apply(double x, double y) { return x + y; } 
    }, 
    MINUS("-") { 
        public double apply(double x, double y) { return x - y; } 
    }, 
    TIMES("*") { 
        public double apply(double x, double y) { return x * y; } 
    }, 
    DIVIDE("/") { 
        public double apply(double x, double y) { return x / y; } 
    }; 
    
    ...
}
```

```java
public enum Operation {
    PLUS("+", (x, y) -> x + y),
    MINUS("-", (x, y) -> x - y),
    TIMES("*", (x, y) -> x * y),
    DIVIDE("/", (x, y) -> x/ y);

    ...
}
```

- 람다는 이름이 없고 문서화도 못 한다. 따라서 코드 자체로 동작이 명확히 설명되지 않거나 코드 줄 수가 많아지면 람다를 쓰지 말아야 한다.
- 람다가 길거나(3줄이상 넘어갈 때) 읽기 어렵다면 더 간단히 줄여보거나 람다식을 쓰지 않는 방향으로 리팩터링하자
- 람다는 자신을 참조할 수 없기 때문에 람다에서 this는 바깥 인스턴스를 가리키고 익명 클래스에서는 자신의 인스턴스를 가리킨다.
- 람다를 직렬화하는 일은 극히 삼가야 한다(익명 클래스의 인스턴스 역시 마찬가지).

# 핵심 정리

- 익명 클래스는 (함수형 인터페이스가 아닌) 타입의 인스턴스를 만들 때만 사용하라.
