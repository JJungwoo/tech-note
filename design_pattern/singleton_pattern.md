# 싱글턴 패턴(Singleton Pattern)

## 정의

싱글턴 패턴은 인스턴스가 하나뿐인 특별한 객체를 만들 수 있게 해주는 패턴으로 만들어진 단 하나의 인스턴스는 어디서든 그 인스턴스만 접근할 수 있다.

## 용도

하나만 있으면 되는 객체로서 스레드 풀, 캐시, 사용자 설정, 레지스트리 설정 처리 객체, 로그 기록용 객체, 프린터나 그래픽 카드 같은 디바이스를 위한 디바이스 드라이버 등 예시가 있다.
<br>
따라서 보통 객체의 인스턴스가 두 개 이상 있으면 문제가 되거나 자원을 불필요하게 사용하는 것을 방지할 때 사용한다.

### 단순 기본 구현 방법

일반적으로 싱글턴 패턴을 구현할 때는 다음과 같이 생성자를 내부 접근제어를 통해 외부에서 함부로 인스턴스를 만들수 없게 구현하여 만들 수 있다.
<br>
이를 통해 사용자는 단 하나의 인스턴스만 접근하고 사용할 수 있다.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

```java
public void static main(String[] args) {
    Singleton instance = Singleton.getInstance();
    System.out.prinltn(instance == Singleton.getInstance()) // true, 동일한 인스턴스
}
```

하지만 위 코드의 getInstance() 메서드는 병렬 기반 프로그래밍에서 서로 다른 스레드가 동시에 첫 인스턴스를 획득할 때 문제가 될 수 있다.

### 병렬 프로그래밍에서 구현 방법

위에서 언급했듯 병렬 프로그래밍에서 싱글턴을 구현하려면 `synchronized` 를 통해 동기화 작업을 처리해줘야 한다.

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() { }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

이를 통해 병렬 프로그래밍의 멀티 스레드 접근에 대한 문제를 해결할 수 있다.

### 이른 초기화(eager initialization) 구현 방법

동기화 처리는 비용에 대한 부담이 다른 방법에 비해 들 수 있다. 따라서 더 빠르게 인스턴스를 할당 받는 방법이 다음과 같다.

```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() { }

    public static Singleton getInstance() {
        return instance;
    }
}
```

해당 방법은 클래스가 로딩되는 과정에서 이미 인스턴스를 미리 만들어 놓기 때문에 이후 인스턴스에 대한 생성 이슈는 모두 해결할 수 있다.
<br>
다만 해당 방법은 해당 인스턴스를 사용하지 않음에도 무조건 미리 만들어 놓기 때문에 인스턴스 자체가 생성에 비용이 많이 든다면 좋지 않은 방법이 될 수 도 있다.

### DCL(Double-Checking Locking) 구현 방법

`synchronized` 에 대한 비용 자체를 더 줄일 수 있는 방법이 있다. `synchronized` 를 메서드 단위로 하지 않고 플럭 단위로 지정해서 동기화 로직의 범위를 더 줄이는 방법이다.
<br>
따라서 이후에 getInstance() 메서드를 사용할 때마다 동기화 처리되던 작업을 이미 인스턴스가 있다면 피할 수 있다.
<br>
대신 이 방법은 `volatile` 키워드를 사용해야 하기 때문에 jdk 1.5 이상부터 사용이 가능하다.


```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() { }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

### static inner 클래스 사용 구현 방법

위 모든 방법 외에 더 효율적으로 구현하는 방법이 존재한다. 해당 방법은 getInstance() 메서드가 호출될 때 내부 클래스가 로딩되어 인스턴스가 생성되기 때문에 멀티스레드에서도 안전하고 인스턴스를 지연 할당 할 수 있다.

```java
public class Singleton {

    private Singleton() { }

    private static class SingletonHolder {
        private static final Singleton instance = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

### 참고

- 인프런 강의 - 코딩으로 학습하는 GoF의 디자인 패턴(백기선)
- Head First Design Patterns - Chapter 5. 싱글턴 패턴


