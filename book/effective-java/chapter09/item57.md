# 지역변수의 범위를 최소화하라

지역변수의 유효 범위를 최소로 줄이면 코드 가독성과 유지보수성이 높아지고 오류 가능성은 낮아진다.

### 지역변수의 범위 줄이기

**지역변수의 범위를 줄이는 가장 강력한 기법은 역시 '가장 처음 쓰일 때 선언하기'다.**

미리 선언부터 해두면 코드가 어수선해져 가독성이 떨어진다. 변수를 실제로 사용하는 시점엔 타입과 초깃값이 기억나지 않을 수도 있다.

### 변수의 초기화

**거의 모든 지역변수는 선언과 동시에 초기화해야 한다.**

초기화에 필요한 정보가 충분하지 않다면 충분해질 때까지 선언을 미뤄야 한다. `try-catch`문은 이 규칙에서 예외다. <br>
변수를 초기화하는 표현식에서 검사 예외를 던질 가능성이 있다면 `try` 블럭 안에서 초기화해야 한다(그렇지 않으면 예외가 블럭을 넘어 메서드에까지 전파된다).

예외로 변수 값을 `try` 블록 바깥에서도 사용해야 한다면(초기화하지 못하더라도) `try` 블럭 앞에서 선언해야 한다.

### 반복문은 되도록이면 `for`, `for-each`문을 사용하자 

`for`문은 반복 변수의 범위가 `for`문 블럭 안으로 제한되기 때문에 반복 변수의 값을 반복문이 종료된 뒤에도 써야 하는 상황이 아니라면 `while`문 보다는 `for`문을 쓰는 편이 낫다.


#### `while` 문 사용 시 발생 가능한 오류 상황

```java
Iterator<Element> i = c.iterator();
while (i.hasNext()) {
    doSomething(i.next);
}
...
Iterator<Element> i2 = c2.iterator();
while (i.hasNext()) {
    doSomething(i2.next);
}
```

위 예시에는 `while`문에서 흔히 발생 가능한 오류가 있다. 그건 i 라는 반복 변수를 활용해서 반복문을 돌고 있는데 `while`문의 조건으로 처음 i를 쓰고 그 다음은 i2를 써야하는데<br>

실수로 똑같이 i를 사용해서 i2는 순회하지 못하게된다. 따라서 이런 실수를 하지 않도록 아래의 `for`문을 활용한 같은 예시를 보여준다.

```java
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
    Element e = i.next;
    ... // e와 i로 무언가를 한다.
}
...
// 다음 코드는 "i를 찾을 수 없다"는 컴파일 오류를 낸다.
for (Iterator<Element> i2 = c2.iterator(); i.hasNext(); ) {
    Element e2 = i2.next;
    ... // e2와 i2로 무언가를 한다.
}
```

`for` 문이 이런 복사 붙여넣기 오류를 줄여주는 또 다른 이유로는 변수 유효 범위가 `for`문 범위와 일치하여 똑같은 이름의 변수를 여러 반복문에서 써도 문제 없다는 점이다.

마지막으로 `for`문이 `while`문 보다 좋은 이유이다.

```java
for (int i = 0, n < expensiveComputation(); i < n; i++) {
    ... // i로 무언가를 한다.
}
```

`for`문을 사용하면 `for`문 내에서 조건과 반복을 모두 처리 가능하여 가독성이 좋다.

#### 반복자를 사용해야 하는 상황이면 `for-each`문 대신 `Iterator`를 활용하자

- `for-each` 문
 
```java
for (Element e : c) {
    ... // e로 무언가를 한다.
}
```

- `Iterator` 활용 전통적인 'for' 문

```java
for (Iterator<Element> i = c.iterator(); i.hasNext(); ) {
    Element e = i.next;
    ... // e와 i로 무언가를 한다.(반복자의 remove 메서드 같은..)
}
```

### 지역변수 범위 최소화하는 마지막 방법

**메서드를 작게 유지하고 한 가지 기능에 집중하는 것이다.**

단순히 메서드를 기능별로 쪼개서 수행 범위 내에서만 지역변수를 사용하자.


- 참고 : 이펙티브 자바 3/E
