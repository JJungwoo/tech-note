
# 상속(Inheritance)

자바에서 상속이란 클래스 간의 **특성(속성과 기능)** 을 물려 받는 시스템이다. 상속을 받는 클래스는 상속하는 클래스의 속성과 기능 즉 변수와 메서드를 사용할 수 있다.(접근 지정자라는 키워드가 있으면 사용할 수 없을 수도 있다.)

프로그래머들은 이 시스템을 통해 더욱 객체 지향 프로그래밍을 할 수 있게 되었다.

## 상속 특징

상속은 부모 클래스와 자식 클래스로 구성된다. 

>**조상 클래스** : 부모(parent) 클래스, 상위(super) 클래스, 기반(base) 클래스
><br>
>**자손 클래스** : 자식(child) 클래스, 하위(sub) 클래스, 파생된(derived) 클래스


![](https://images.velog.io/images/host92/post/fa27e2f0-664f-42c7-9fe2-7c2cde4e7795/image.png)

```java
class Parent {
    String name;
    int age;
}

class Child extends Parent {
    void play() {
        ...
    }
}
```

위 그림과 코드를 설명하자면 `Parent` 클래스는 부모 클래스로서 name, age의 속성을 갖고 있고 `Child`는 `Parent`를 **상속(extends)** 받아 `Parent` 클래스의 name, age를 사용할 수 있다.

>
- 생성자와 초기화 블럭은 상속되지 않는다. **멤버(변수, 메서드)만 상속된다.**
- 자식 클래스의 멤버 개수는 부모 클래스보다 항상 같거나 많다.
- 자바에서는 자식 클래스가 **무조건 하나의 부모 클래스만 상속(extends)** 받을 수 있다.**(단일 상속)**

# 제어자(modifier)

제어자(modifier)는 클래스, 변수 또는 메서드의 선언부에 함께 사용된다.
제어자는 접근 제어자와 그 외의 제외자로 나눠진다.

- 접근 제어자 : public, protected, default, private
- 그 외 : static, final, abstract, native, transient, synchronized, volatile, strictfp


## 접근 제어자(access modifier)

접근 제어자는 멤버 또는 클래스에 사용되어 해당하는 멤버 또는 클래스를 외부에서 접근하지 못하도록 제한하는 역할을 한다.

- **접근 제어자가 사용될 수 있는 곳 - 클래스, 멤버변수, 메서드, 생성자**
  - **public** : 접근 제한 없이 어디에서든 접근 가능하다.
  - **protected** : 같은 패키지 내에서 그리고 다른 패키지의 자식 클래스에서 접근이 가능하다.(상속 받은 자식은 모두 접근 가능)
  - **default** : 같은 패키지 내에서만 접근이 가능하다. 접근 제어자를 선언하지 않으면 자동으로 default로 선언된다.
  - **private** : 같은 클래스 내에서만 접근이 가능하다.


|제어자|같은 클래스|같은 패키지|자식 클래스| 전 체 |
|---|---|---|---|---|
|**public**|접근 가능|접근 가능|접근 가능|접근 가능|
|**protected**|접근 가능|접근 가능|접근 가능|-|
|**(default)**|접근 가능|접근 가능|-|-|
|**private**|접근 가능|-|-|-|

> **public > protected > (default) > private**

#### 접근 제어자를 이용한 캡슐화

클래스나 멤버, 주로 멤버에 접근제어자를 사용하는 이유는 클래스 내부에 선언된 데이터를 외부에 노출되지 않게하여 보호하기 위해서이다. 이는 **데이터가 비밀번호 같은 보안이 필요한 데이터를 함부로 변경할 수 없도록 접근을 제어하는 것**이고 개발자에게는 **개발에 굳이 알 필요가 없는 멤버나 메서드에 접근하지 않도록 정보를 제어한다**. 이것을 **데이터 감추기(data hiding)** 즉 객체지향개념으로 **캡슐화(encapsulation)**이라 한다.

>
- 접근 제어자를 사용하는 이유
  - 외부로부터 데이터를 보호하기 위해서
  - 외부에는 불필요한 내부적으로만 사용되는 부분을 감추기 위해서


### final 키워드

final은 **클래스, 메서드, 멤버변수, 지역변수**에 사용될 수 있다.

|제어자|대상|의미|
|---|---|---|
|**final**|클래스|변결될 수 없는 클래스, 확장될 수 없는 클래스로 정의(상속 불가)|
||메서드|변결될 수 없는 메서드, final로 지정된 메서드는 오버라이딩을 통해 재정의 불가|
||멤버변수 & 지역변수|변수 앞에 final이 붙으면 값을 변경할 수 없는 상수로 정의|

```java
final class FinalTest {           // 조상이 될 수 없는 클래스
    final int MAX_SIZE = 10;      // 값을 변경할 수 없는 멤버변수(상수)
    
    final void getMaxSize() {     // 오버라이딩 할 수 없는 메서드(변경불가)
        final int LV = MAX_SIZE;  // 값을 변경할 수 없는 지역변수(상수)
        return MAX_SIZE;
    }
}
```

# super 키워드

### super - 부모 클래스의 멤버 변수 사용

super는 자식 클래스에서 부모 클래스로부터 상속 받은 멤버를 참조하는데 사용되는 참조 변수이다. 멤버변수와 지역변수의 이름이 같을 때 this를 붙여서 구별했듯이 상속 받은 벰버와 자신의 멤버와 이름이 같을 때는 super를 붙여서 구별할 수 있다.

#### super 예제 코드와 실행 결과

```java
class SuperTest {
    public static void main(String args[]) {
        Child c = new Child();
        c.method();
    }
}
class Parent {
    int x = 10;
}
class Child extends Parent {
    int x = 20;
    void method() {
        System.out.println("x=" + x); // 나의 멤버 변수
        System.out.println("this.x=" + this.x); // 나의 멤버 변수
        System.out.println("super.x=" + super.x); // 부모의 멤버 변수
    }
}
```

```
x=20
this.x=20
super.x=10
```

### super() - 부모 클래스의 생성자

this()와 마찬가지로 super() 역시 생성자로서 부모 클래스의 생성자를 자식 클래스에서 호출할 때 사용한다. 자식 클래스에서 부모 클래스의 멤버 변수를 접근할 수 있는 이유 역시 super()를 통해 부모 클래스의 멤버 변수를 초기화해주기 때문이다.

> Object 클래스를 제외한 모든 클래스의 생성자 첫 줄에 생성자, this() 또는 super(),를 호출해야 한다. 그렇지 않으면 컴파일러가 자동으로 `super();`를 생성자의 첫 줄에 삽입한다.

>**Object** 클래스는 **모든 클래스의 최상위에 있는 부모 클래스**로 다른 클래스로부터 상속 받지 않는 모든 클래스는 자동으로 Object 클래스를 상속 받는다.

```java
public class PointTest {
    public static void main(String[] args) {
        Point3D p = new Point3D(1, 2, 3);
        System.out.println("x=" + p.x);
        System.out.println("y=" + p.y);
        System.out.println("z=" + p.z);
    }
}

class Point {
    int x = 10;
    int y = 20;

    public Point(int x, int y) {
        // 해당 라인에 Object 클래스의 생성자를 호출하는 과정이 생략되어 있지만
        // 컴파일러가 컴파일과정에 자동으로 삽입한다.
        // super(); <- 컴파일러에 의해 Object 클래스의 생성자 호출(생략 가능)
        this.x = x;
        this.y = y;
    }
}

class Point3D extends Point {
    int z = 30;

    Point3D() {
        // Point3D(int x, int y, int z)를 호출한다.
        this(100, 200, 300);
    }
    public Point3D(int x, int y, int z) {
        super(x, y); // Point(int x, int y)를 호출한다.
        this.z = z;
    }
}
```

```
x=1
y=2
z=3
```


> 위 예시에서 부모 클래스의 super() 를 통해 부모 클래스의 생성자를 호출하는데 만약 부모 클래스에 super() 즉 default 생성자가 없으면 컴파일 에러가 발생한다.

# 오버라이딩

오버라이딩이란 자식 클래스로부터 상속받은 메서드의 내용을 변경하는 것을 말한다. 

### 오버라이딩 특징

- 자식 클래스에서 오버라이딩하는 메서드는 부모 클래스의 메서드와
  - **이름이 같아야 한다.**
  - **매개변수가 같아야 한다.**
  - **반환타입이 같아야 한다.**


- 접근 제어자는 **부모 클래스의 메서드보다 좁은 범위로 변경 할 수 없다.**

```java
class Parent {
    protected void parentMethod() {
        ...
    }
}

class Child extends Parent {
    // 현재 부모 클래스의 접근 제어자가 protected라서 protected, public 접근 제어자만 선언 가능하다.
    void parentMethod() {
        ...
    }
}
```

- 부모 클래스의 메서드보다 **많은 수의 예외 그리고 상위 예외 클래스를 선언할 수 없다.**

```java
class Parent {
    void parentMethod() throws IOException, SQLException {
        ...
    }
}

class Child extends Parent {
    // 만약 부모 클래스의 예외보다 더 많이 예외를 선언하면 에러 발생
    // 그리고 부모 클래스의 예외보다 더 상위 클래스의 예외를 선언 불가능 즉 Exception 선언 불가능!
    void parentMethod() throws IOException {
        ...
    }
}
```

#### 부모 클래스의 메서드를 자식 클래스에서 오버라이딩할 때
1. 접근 제어자를 **부모 클래스의 메서드보다 좁은 범위로 변경할 수 없다.**
2. 예외는 **부모 클래스의 메서드보다 많이 선언할 수 없다.**
3. 인스턴스 메서드를 static 메서드로 또는 그 반대로 변경할 수 없다.**(static 메서드는 오버라이딩 불가!)**

>
부모 클래스와 자식 클래스에 static 키워드로 동일한 메서드를 선언하면 오버라이딩이 아닌 각 클래스에 클래스 메서드를 선언한 것이다. 따라서 각 클래스의 메서드를 생성하여 사용하기 때문에 `클래스.static메서드()`와 같이 호출하는 것이 혼동을 줄여준다.

### 오버로딩과 오버라이딩

- **오버로딩(overloading)**: 기존에 없는 **새로운 메서드를 정의**하는 것(new)
- **오버라이딩(overriding)**: 상속받은 메서드의 **내용을 변경**하는 것(change, modify)

오버로딩과 오버라이딩이 용어가 비슷해서 많이 헷갈리는데, 오버로딩은 메서드를 다시 새로 생성한다 그리고 오버라이딩은 기존 내용을 엎어쓴다라고 용어를 생각하면 이해하기 쉽다.

>override의 사전적 의미는 '~위에 덮어쓰다(overwrite)'이다.

#### 오버로딩과 오버라이딩 예시

```java
class Parent {
    void parentMethod();
}
class Child extends Parent {
    void parentMethod() {}       // 오버라이딩
    void parentMethod(int i) {}  // 오버로딩
    
    void childMethod() {}
    void childMethod(int i) {}   // 오버로딩
}
```

### 메소드 디스패치 (Method Dispatch)

...

# 정리

![](https://images.velog.io/images/host92/post/c2581484-9af5-410e-b030-f24dda01a4aa/image.png)


------------------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
해당 글을 참고하시거나 퍼가실 때는 출처 링크 부탁드립니다 :)

참고
1. [백기선-자바스터디](https://github.com/whiteship/live-study)
2. 이것이 자바다 - 신용권의 Java 프로그래밍 정복
3. 자바의 정석 책 3rd Edition
