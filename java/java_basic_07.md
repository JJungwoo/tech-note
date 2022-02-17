인터페이스는 일종의 추상 클래스이다. 인터페이스는 추상클래스처럼 추상메서드를 갖지만 추상클래스보다 추상화 정도가 높아서 추상클래스와 달리 몸통을 갖춘 일반 메서드 또는 멤버변수를 구성원으로 가질 수 없다. 오직 추상메서드와 상수만 멤버로 가질 수 있다.

- **추상 클래스** : 부분적으로만 완성된 '미완성 설계도'
- **인터페이스** : 밑그림만 그려져 있는 '기본 설계도'

> 
>JDK1.8 부터 default 메서드와 static 메서드가 지원된다.<br>
>JDK1.9 부터 private 메서드가 지원된다.


## 인터페이스 정의

```java
public(or default) interface 인터페이스이름 {
    public static final 타입 상수이름 = 값;
    타입 상수이름 = 값; // public static final 생략
    public abstract 타입 메서드이름 (매개변수목록);
    타입 메서드이름 (매개변수목록); // public abstract 생략
    // default 메서드
    default 타입 메서드이름(매개변수목록) { ... }
    // static 메서드
    static 타입 메서드이름(매개변수목록) { ... }
}
```

- 인터페이스 멤버 제약사항
  - 모든 멤버변수는 public static final 이어야 하며, 이를 생략할 수 있다.
  - 모든 메서드는 public abstract 이어야 하며, 이를 생략할 수 있다. 
    단, static 메서드와 default 메서드는 예외(JDK1.8부터) + private 메서드 포함(JDK1.9부터)

## 인터페이스의 장점

- 개발시간을 단축시킬 수 있다.
- 표준화가 가능하다.
- 서로 관계없는 클래스들에게 관계를 맺어 줄 수 있다.
- 독립적인 프로그래밍이 가능하다.

## 인터페이스의 상속

인터페이스는 인터페이스로부터만 상속받을 수 있으며 클래스와는 달리 다중상속, 즉 여러개의 인터페이스로부터 상속 받는 것이 가능하다.
>인터페이스는 클래스와 달리 Object 클래스와 같은 최고 부모 클래스가 없다.

```java
interface Movable {
    /** 지정된 위치(x, y)로 이동하는 기능의 메서드 */
    void move(int x, int y);
}
interface Attackable {
    /** 지정된 대상(u)을 공격하는 기능의 메서드 */
    void attack(Unit u);
}
// 다중 상속
interface Fightable extends Movable, Attackable {
    ...
}
```

## 인터페이스의 구현 

- 인터페이스 구현 예시

```java
public class FighterTest {
    public static void main(String[] args) {
        Fighter f = new Fighter();

        if (f instanceof Unit)
            System.out.println("f는 Unit 클래스의 자식입니다.");

        if (f instanceof Fightable)
            System.out.println("f는 Fightable 인터페이스를 구현했습니다.");

        if (f instanceof Movable)
            System.out.println("f는 Movable 인터페이스를 구현했습니다.");

        if (f instanceof Attackable)
            System.out.println("f는 Attackable 인터페이스를 구현했습니다.");

        if (f instanceof Object)
            System.out.println("f는 Object 클래스의 자식입니다.");

    }
}

class Fighter extends Unit implements Fightable {

    @Override
    public void move(int x, int y) {
        /* 내용 생략 */
    }

    @Override
    public void attack(Unit u) {
        /* 내용 생략 */
    }
}

class Unit {
    int currentHP;  // 유닛의 체력
    int x;          // 유닛의 x좌표
    int y;          // 유닛의 y좌표
}

interface Fightable extends Movable, Attackable {}
interface Movable { void move(int x, int y); }
interface Attackable { void attack(Unit u); }
```
- 실행 결과
```
f는 Unit 클래스의 자식입니다.
f는 Fightable 인터페이스를 구현했습니다.
f는 Movable 인터페이스를 구현했습니다.
f는 Attackable 인터페이스를 구현했습니다.
f는 Object 클래스의 자식입니다.
```

> 
인터페이스의 이름에는 주로 Fightable과 같이 '~을 할 수 있는'의 의미인 'able'로 끝나는 것들이 많은데, 그 이유는 **어떠한 기능 또는 행위를 하는데 필요한 메서드를 제공한다는 의미를 강조**하기 위해서이다. 또한 그 인터페이스를 구현한 클래스는 **'~를 할 수 있는' 능력을 갖추었다는 의미**이기도 하다.


## 인터페이스의 다형성[3]

다형성(Polymorphism)은 한 객체가 여러가지(poly) 모습(morph)을 갖는다는 것을 의미한다. 여기서 모습이란 타입을 뜻하는데, 즉 **다형성이란 한 객체가 여러 타입을 가질수 있다는 것을 뜻한다.**

아래의 예시 코드를 통해 다형성을 좀 더 구체적으로 설명한다.

```java
class Fighter extends Unit implements Fightable {
    public void fight() {
        /* 내용 생략 */
    }

    @Override
    public void move(int x, int y) {
        /* 내용 생략 */
    }
}

class Unit {
    int currentHP;  // 유닛의 체력
    int x;          // 유닛의 x좌표
    int y;          // 유닛의 y좌표
}

interface Movable { 
    void move(int x, int y); 
}
```

위에서 예를 들었던 Fighter 클래스를 활용하여 다형성을 설명한다. Fighter 클래스는 **Unit을 상속 받고(=Unit의 특징을 갖고 있고) Movable을 구현한다.(=Movable의 기능을 할 수 있다.)**

```java
Fighter ft = new Fighter();
ft.currentHP; // Unit에 정의/구현된 변수 사용
ft.move();    // Movable에 정의되고 Fighter에 구현된 메서드 실행
```

Fighter 타입의 객체를 Unit 타입이나 Movable 타입에 할당하는 것도 가능하다.

```java
Fighter ft = new Fighter();

Unit u = ft;  // Fighter 객체는 Unit 타입도 된다.
u.currentHP;

Movable m = ft;  // Fighter 객체는 Movable 타입도 된다.
m.move();
```

따라서 **Fighter 타입의 객체는 여러가지 모습들(Unit, Movable)을 갖을 수 있다.**

>
```java
Fightable method() {
    ...
    Fighter f = new Fighter();
    return f;
    // 위 문장은 한줄로 다음과 같다. -> return new Fighter();
}
```
리턴타입이 인터페이스라는 것은 메서드가 해당 인터페이스를 **구현한 클래스의 인스턴스를 반환**한다는 것을 의미한다.

### 인터페이스를 익명 클래스로 활용

인터페이스를 익명 클래스로 활용하는 예시를 간단히 작성하였다. Calculator(계산기) 클래스가 Calculable(계산할 수 있는) 인터페이스를 활용하여 덧셈(sum) 메서드를 구현하는 익명 클래스 예시이다.

- 인터페이스 익명 클래스 구현 예시

```java

public class InterfaceTest {
    public static void main(String[] args) {
        System.out.println(Calculable.OPERAT_NAME + 
        " 결과는 :" + 
        Calculator.calculate(1, 2, new Calculable() {
            @Override
            public int sum(int a, int b) {
                return a + b;
            }
        }));
    }
}

class Calculator {
    public static int calculate(int a, int b, Calculable c) {
        return c.sum(a, b);
    }
}

interface Calculable {
    String OPERAT_NAME = "더하기";
    int sum(int a, int b);
}
```

- 실행 결과

```
더하기 결과는 :3
```

위의 인터페이스를 익명클래스로 활용하는 예시 중 가장 자주 쓰이는 인스턴스가 Comparator가 존재한다.

## default 메서드와 static 메서드

인터페이스는 추상 메서드만 선언이 가능하였는데, **JDK 1.8부터 default 메서드와 static 메서드도 선언할 수 있게 되었다.**

### default 메서드와 static 메서드 정의와 사용 예시

```java
interface MyInterface {
    void method();
    void newMethod();  // 추상 메서드
    // default 메서드 (default 키워드 생략 불가, public은 생략 가능)
    default void newDefaultMethod() { /* 구현 생략 */ } 
    static void newStaticMethod() { /* 구현 생략 */ }
}
```

- default 메서드와 static 메서드 사용 예시

```java
public class InterfaceMethodTest {
    public static void main(String[] args) {
        InterfaceTestClass i = new InterfaceTestClass();
        i.method();
        i.defaultMethod();
        MyInterface.staticMethod();
    }
}

class InterfaceTestClass implements MyInterface {
    public void method() {
        System.out.println("method() in InterfaceTestClass");
    }
}

interface MyInterface {
    default void defaultMethod() {
        System.out.println("default mothod in MyInterface");
    }

    static void staticMethod() {
        System.out.println("staticMethod() in MyInterface");
    }
}
```

- 실행 결과

```
method() in InterfaceTestClass
default mothod in MyInterface
staticMethod() in MyInterface
```


### default 메서드 사용 시 주의사항

JDK1.8 버전 이후부터 인터페이스에 추가된 default 메서드 기능으로 인터페이스에 default 메서드와 상속 받는 클래스의 메서드가 이름이 중복되어 충돌이 발생하는 경우가 발생하게 되었다. 이런 충돌 상황을 해결하는 규칙으로 다음 내용이 있다.

- **1. 여러 인터페이스의 default 메서드 간의 충돌**
    - 인터페이스를 구현한 클래스에서 default 메서드를 오버라이딩해야 한다.
- **2. default 메서드와 부모 클래스의 메서드 간의 충돌**
    - 부모 클래스의 메서드가 상속되고 default 메서드는 무시한다.

## private 메서드[4]

JDK1.9 부터 인터페이스에 private 메서드를 제공한다. 

### private 메서드 정의와 사용 예시

인터페이스에서 제공하는 private 메서드는 static 또는 non-static으로 선언할 수 있다. 

```java
interface MyInterface {
    ...
    private void newPrivateMethod() { /* 구현 생략 */ }
    private static void newPrivateMethod() { /* 구현 생략 */ }    
}
```

- private 메서드 사용 예시

인터페이스는 객체를 생성하지 않기 때문에 private 메서드를 사용하려면 default 메서드 또는 static public 메서드에 호출하여 사용한다.**(캡슐화하여 private 메서드 사용)**

```java
public class InterfaceMethodTest {
    public static void main(String[] args) {
        InterfaceTestClass i = new InterfaceTestClass();
        i.method();
        i.defaultMethod();
        MyInterface.staticMethod();
    }
}

class InterfaceTestClass implements MyInterface {
    public void method() {
        System.out.println("method() in InterfaceTestClass");
    }
}

interface MyInterface {
    default void defaultMethod() {
        System.out.println("default mothod in MyInterface");
        privateMethod();
    }
    private void privateMethod() {
        System.out.println("privateMethod in MyInterface");
    }

    static void staticMethod() {
        System.out.println("staticMethod() in MyInterface");
        privateStaticMethod();
    }
    private static void privateStaticMethod() {
        System.out.println("privateStaticMethod() in MyInterface");
    }
}
```
- 실행 결과
```
method() in InterfaceTestClass
default mothod in MyInterface
privateMethod in MyInterface
staticMethod() in MyInterface
privateStaticMethod() in MyInterface
```

따라서 인터페이스에서 private 메서드를 제공하는 이유는 캡슐화를 하기 위해서 이다.

-----------------------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
해당 글을 참고하시거나 퍼가실 때는 출처 링크 부탁드립니다 :)

참고
1. [백기선-자바스터디](https://github.com/whiteship/live-study)
2. 이것이 자바다 - 신용권의 Java 프로그래밍 정복
3. 개발자가 반드시 정복해야할 객체 지향과 디자인 패턴
4. [Private Methods in Java Interfaces](https://www.baeldung.com/java-interface-private-methods)
5. 자바의 정석 책 3rd Edition

