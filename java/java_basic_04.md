# O.O.P! 객체 지향 프로그래밍!
현실 세계에서 어떤 제품을 만들 때, 부품을 먼저 개발하고 이 부품들을 하나씩 조립해서 완성된 제품을 만들 듯이 소프트웨어를 개발할 때에도 부품에 해당하는 객체들을 먼저 만들고 이것들을 하나씩 조립해서 완성된 프로그램을 만드는 기법을 **객체 지향 프로그래밍(OOP: Object Oriented Programming)**이라 한다.

>
- 객체지향언어는?
**1. 코드의 재사용성이 높다.**
새로운 코드를 작성할 때 기존의 코드를 이용하여 쉽게 작성할 수 있다.
**2. 코드의 관리가 용이하다.**
코드 간의 관계를 이용해서 적은 노력으로 쉽게 코드를 변경할 수 있다.
**3. 신뢰성이 높은 프로그래밍을 가능하게 한다.**
제어자와 메서드를 이용해서 데이터를 보호하고 올바른 값을 유지하도록 하며, 코드의 중복을 제거하여 코드의 불일치로 인한 오동작을 방지할 수 있다.

## 객체란?
객체 지향 프로그래밍에서 기본 실행 흐름 또는 데이터 단위가 되는 정보로 **속성(필드, 변수)**과 **기능(메서드)**으로 구성되어 있다. **인스턴스(Instance)**, **오브젝트(Object)**도 객체와 같은 말이다. 간단하게 클래스를 메모리에 할당(**new**)했을 때를 객체라고 부른다.

# 클래스
클래스란 객체가 생성되기전 자바 소스 코드(*.java) 내부 코드로 객체를 만들기 위한 **설계도(정의)**이다.

## 클래스의 구성
  - **필드(field)** : 클래스의 속성 정보
    - **인스턴스 변수(instance variable)** : 클래스가 생성(new)될 때부터 GC에 의해 해제되기 전까지 메모리에 할당되어 있는 정보로 클래스 내부 생성자 혹은 메서드에서 모두 접근이 가능하다.
    - **클래스 변수(static variable)** : 인스턴스 변수처럼 클래스 내부 생성자 혹은 메서드에서 모두 접근이 가능하지만 클래스가 생성(new)되기 전에 이미 프로그램이 실행될 때부터 프로그램 종료시까지 메모리에 할당되어 사용할 수 있다.
  - **생성자(constructor)** : 클래스가 생성(new)될 때, 클래스 내부 필드들에 대한 초기화를 해주는 메서드이다.
  - **메서드(method)** : 클래스의 특정 작업을 수행하는 일련의 문장들을 하나로 묶은 작업 단위이다. 특정 값을 입력 받아서 작업을 수행한 뒤 값을 반환할 수 있다. 
  - **초기화 블럭(initialization block)**
    - **클래스 초기화 블럭(static initialization block)** : 클래스(static) 변수들을 초기화하는 블럭이다.
    - **인스턴스 초기화 블럭(instance initialization block)** : 인스턴스 변수들을 초기화하는 블럭이다.
  - **내부 클래스(inner class)** : 클래스 내에 선언되는 클래스이다. 인스턴스 클래스, 스태틱 클래스, 지역 클래스, 익명 클래스가 있다.
  
>
인스턴스 변수는 인스턴스가 생성될 때 마다 생성되므로 인스턴스 마다 각기 다른 값을 유지하지만 클래스 변수는 모든 인스턴스가 **하나의 저장 공간(메서드 영역)**을 공유하므로 항상 공통된 값을 갖는다.(=**공유 변수**)

## 클래스 구성 코드 설명

```java
class ClassTest {
    // field
    static int staticValue;    // 1. 클래스 변수
    int instanceValue;         // 2. 인스턴스 변수

    // 3. 클래스 초기화 블럭
    static {
        staticValue = 0;
    }
    
    // 4. 인스턴스 초기화 블럭
    {
        instanceValue = 0
    }
    
    // 5. 생성자
    public ClassTest(int instanceValue) {
        this.instanceValue = instanceValue;
    }
    
    // 6. 클래스 메서드
    public static void staticMethod() {
        ...
    }
    
    // 7. 인스턴스 메서드
    public void instanceMethod() {
        ...
    }
    
    // 내부 클래스
    class InnerClass {
    
    }

}
```

위 예시는 클래스 구성 요소들을 작성한 코드로 내부 클래스를 제외한 각각의 요소들의 생성 시점을 순서대로 나열하면 먼저 클래스 생성 이전 순서는 `1->6->3` 이다. 

만약 `ClassTest` 클래스를 객체로 생성하면

```java
// ClassTest 객체 생성
ClassTest ct = new ClassTest(1);
```

클래스 생성 이후 순서는 `2->7->4->5` 이다.


- 클래스 초기화
  - 1. 클래스 변수 -> 6. 클래스 메서드 -> 3. 클래스 초기화 블럭
- 인스턴스 초기화
  - 2. 인스턴스 변수 -> 7. 인스턴스 메서드 -> 4. 인스턴스 초기화 블럭 -> 5. 생성자


따라서 각 변수와 메서드는 먼저 메모리에 할당되고 이후 초기화 블럭 진행 -> 생성자 호출 순으로 객체의 생성이 마무리 된다.

>
멤버변수의 초기화 방법
1. **명시적 초기화**(explicit initialization)
2. **초기화 블럭**(initialization block)
3. **생성자**(constructor)
순서는 1, 2, 3 순으로 명시적 초기화 이후 초기화 블럭이 다시 초기화하고 마지막으로 생성자에 의해 객체 생성 시에 초기화가 이뤄진다. 명시적 초기화는 변수 선언 즉시 바로 값을 할당하는 것이다.
ex) int value = 0;


## 생성자의 사용 방법

모든 클래스에는 반드시 하나 이상의 생성자가 정의되어 있어야 한다. 만약 생성자가 보이지 않는다면 컴파일러가 제공하는 기본 생성자이다.(**기본 생성자가 추가되는 경우는 클래스에 생성자가 하나도 정의되지 않을 때만 추가가 된다.**)

- 생성자 정의 규칙
  - 생성자의 이름은 클래스의 이름과 같아야 한다.
  - 생성자는 리턴 값이 없다.

> 모든 생성자는 리턴 값이 없기 때문에 생성자에 void 키워드를 생략할 수 있다.

### 기본 생성자

```java
class People {
    String name;
    int age;
}

People p = new People();
```

위 예시에서 People 객체를 생성했을 때 People 클래스는 현재 생성자가 정의되어 있지 않기 때문에 기본 생성자가 호출된다.

### 매개변수가 있는 생성자

```java
class People {
    String name;
    int age;
    
    People (String n) {
        name = n;
    }
}

People p = new People("홍길동");
```

위 예시는 People 객체 생성 시 "홍길동"이라는 이름으로 name을 초기화하는 생성자를 호출한 예시이다.

### this(), this 키워드

this 키워드는 클래스 내부에서 자신(생성된 객체)을 접근할 때 사용한다. 즉 위 People 클래스 내부에서 this.name 을 하면 현재 생성한 객체의 name 을 말한다.

이를 활용하여 생성자를 호출할 때 인자(argments)값이 name으로 People 클래스의 인스턴스 변수와 이름이 같을 때 에러와 혼동을 피하기 위해 사용할 수 있다.


```java
class People {
    String name;
    int age;
    
    People (String name) {
        this.name = name;
    }
}

People p = new People("홍길동");
```

그리고 this는 클래스 내부에서 객체의 생성자로서 사용될 수 있다.

```java
class People {
    String name;
    int age;

    People () {
    }
    People (String name) {
        this(name, "20");
    }
    People (String name, int age) {
        this.name = name;
        this.age = age;
    }
}

People p = new People("홍길동");
People p = new People("홍길동", "21");
```

위 예시를 통해 생성자를 다양하게 호출이 가능하고 this 키워드를 통해 이를 더 활용이 가능한 것을 확인할 수 있다.


## static 키워드

### 클래스(static) 변수
클래스 변수는 위에서 말한 내용처럼 프로그램 실행 시에 이미 메모리에 할당되어 있기 때문에 객체를 따로 생성하지 않아도 클래스를 통해 접근이 가능하다.

```java
class StaticTest {
    static int value;
    public static void testStaticMethod() {
       ...
    }
}

... 

public static void main(String[] args) {
    // StaticTest 클래스를 생성하지 않아도 사용이 가능
    StaticTest.value = 7;
    StaticTest.testStaticMethod();
}
```

### 클래스(static) 메서드 주의 사항
위에서 언급했듯이 static 키워드가 들어간 함수나 변수, 이후 과정인 클래스는 모두 main 메서드가 동작하기 전에 클래스로더에 의해 프로그램 구동 시 가장 먼저 메모리에 할당되어 사용할 수 있는 정보이다. 

그렇기 때문에 스태틱 메서드의 블럭에서는 인스턴스 변수를 접근할 수 없다. 다음의 예시를 통해 더 자세히 설명한다.

```java
class StaticTest {
    int value = 0;
    public static void testStaticMethod() {	// 스태틱 메서드
        value = 1;	// 인스턴스 변수 -> 컴파일 에러 발생!
    }
}
```

위 예시를 통해 스태틱 메서드에서 인스턴스 변수를 접근했을 때 컴파일러에서 에러를 발생시키는 것을 확인할 수 있다. 스태틱 메서드는 main 메서드 호출 전에 이미 메모리에 할당되어 있고 인스턴스 변수는 해당 클래스가 객체로서 할당될 때 메모리에 할당되기 때문에 스태틱 키워드가 선언된 변수나 함수가 메모리에 할당된 후 실행될 때 인스턴스 변수는 메모리에 없기 때문에 문제가 발생한다.(인스턴스 메서드 역시 마찬가지로 스태틱 메서드에서 호출 불가능하다.)

>
- **메서드 내에서 인스턴스 변수를 사용하지 않는다면, static을 붙이는 것을 고려한다.**
메서드에 static 키워드가 선언되면 메서드를 찾는 과정이 인스턴스 메서드보다 짧기 때문에 메서드 호출 시간이 짧아져 성능이 향상된다.


# 내용 정리
![](https://images.velog.io/images/host92/post/d2f13234-9b05-4229-846e-ed230a7750db/java-gitmind04.png)

----------------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
해당 글을 참고하시거나 퍼가실 때는 출처 링크 부탁드립니다 :)

참고
1. [백기선-자바스터디](https://github.com/whiteship/live-study)
2. 이것이 자바다 - 신용권의 Java 프로그래밍 정복
3. 자바의 정석 책 3rd Edition
