# 비검사 경고를 제거하라

제네릭을 사용하면 비검사 형변환 경고, 비검사 메서드 호출 경고, 비검사 매개변수화 가변인수 타입 경고, 비검사 변환 경고 등 수많은 컴파일 경고를 볼 수 있다.
<br><br>
다음의 예시를 통해 제네릭 사용에서 발생하는 비검사 경고를 쉽게 확인할 수 있다.

```java
Set<Lark> exaltation = new HashSet();
```

컴파일 시 아래와 같이 컴파일러가 잘못된 부분을 알려준다.

```java
$ javac WarningTest.java -Xlint:unchecked 
WarningTest.java:8: warning: [unchecked] unchecked conversion
        Set<String> set = new HashSet(); 
                          ^                           
  required: Set<String> 
  found: HashSet 
1 warning
```

- 해결 방법
  - javac에 **-Xlint:uncheck 옵션** 추가
  - jdk7부터 지원하는 **다이아몬드 연산자(<>)** 추가
  ```java
  Set<Lark> exaltation = new HashSet<>();
  ```

**할 수 있는 한 최대한 모든 비검사 경고를 제거해라!**
<br>
=> 그러면 타입 안전성 보장과 런타임에서 ClassCastException 발생을 방지할 수 있다.

### @SuppressWarnings 애너테이션

경고를 제거할 수는 없지만 타입 안전하다고 확신할 수 있다면 **@SuppressWarnings("unckecked") 애너테이션**을 달아 경고를 숨기자.
>안전하다고 검증된 비검사 경고를(숨기지 않고) 그대로 두면 진짜 문제를 알리는 새로운 경고가 나와도 눈치채지 못할 수 있다

<br>

@SuppressWarnings 애너테이션은 개별 지역변수 선언부터 클래스 전체까지 어떤 선언에도 달 수 있다.
>@SuppressWarnings 애너테이션은 항상 가능한 좁은 범위에 적용하자<br>
>범위를 넓게 잡아 심각한 경고를 놓칠 수 있는 경우가 있으니 절대로 클래스 전체에 적용해서는 안된다!

<br>

- @SuppressWarnings 애너테이션 활용 불가 상황 예시

```java
public <T> T[] toArray(T[] a) {
    if (a.length < size)
        return (T[]) Arrays.copyOf(elements, size, a.getClass()); // 여기서 경고 발생
    System.arraycopy(elements, 0, a, 0, size);
    if (a.length > size)
        a[size] = null;
    return a;
}
```

- 컴파일 시 경고 메시지

```java
ArrayList.java:305: warning: [unchecked] unchecked cast 
        return (T[]) Arrays.copyOf(elements, size, a.getClass()); 

  required: T[] 
  found: Object[]
```

애너테이션은 선언에만 달 수 있으니 return 문에는 @SuppressWarnings 애너테이션을 다는게 불가능하다.

따라서 이런 경우에는 반환값을 담을 지역변수를 하나 선언하고 그 변수에 애너테이션을 다는 방법으로 해결할 수 있다.

- 지역변수 추가한 해결 방법
```java
public <T> T[] toArray(T[] a) { 
  if (a.length < size) { 
    // 생성한 배열과 매개변수로 받은 배열의 타입이 모두 T[]로 같으므로 
    // 올바른 형변환이다. 
    @SuppressWarnings("unchecked") T[] result = 
        (T[]) Arrays.copyOf(elements, size, a.getClass()); 
    return result; 
  } 
  System.arraycopy(elements, 0, a, 0, size); 
  if (a.length > size) 
    a[size] = null; 
  return a; 
}
```

**@SuppressWarnings 애너테이션을 사용할 때면 그 경고를 무시해도 안전한 이유를 항상 주석으로 남겨야 한다**

# 핵심 정리
- 비검사 경고는 중요하니 무시하지 말자
- 모든 비검사 경고는 런타임에 ClassCastException을 일으킬 수 있는 잠재적 가능성이 있으니 최대한 제거해라
- 경고를 없애지 못하겠다면 안전함을 증명하고 @SuppressWarnings 애너테이션을 사용해 경고를 숨기고 

