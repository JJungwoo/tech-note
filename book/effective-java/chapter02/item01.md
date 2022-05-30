# 생성자 대신 정적 팩터리 메서드를 고려하라

- 자바에서 클래스의 인스턴스를 얻는 방법
    - public 생성자
    - 정적 팩터리 메서드(static factory method)


- 정적 팩터리 메서드 예시

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

## 정적 팩터리 메서드 장점

### 1) 별도의 이름을 가질 수 있다.

정적 팩터리 메서드는 이름을 잘 지어서 반환될 객체의 특성을 쉽게 묘사할 수 있다.

ex) BigInteger(int, int, Random) 보다 BigInteger.probablePrime 이라는 정적 팩터리 메서드를 통해 '값이 소수인 BigInteger를 반환한다'는 의미를 더 잘 묘사할 수 있다.

### 2) 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

불변 클래스(immutable class)에 대해 인스턴스를 미리 만들어 놓거나 생성한 인스턴스에 대해 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.

ex) Boolean.valueOf(boolean) 메서드는 객체를 아예 생성하지 않아 자주 사용되는 상황에 성능을 끌어올릴 수 있다. +플라이웨이트 패턴(Flyweight pattern)도 이와 비슷한 기법

### 3) 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

정적 팩터리 메서드를 통해 반환할 객체의 클래스를 자유롭게 선택할 수 있는 '유연성'을 갖을 수 있다.

### 4) 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

정적 팩터리 메서드는 객체를 반환할 때, 반환 타입의 하위 타입이기만 하면 어떤 클래스든 반환할 수 있다.
즉 다형성의 특성을 통해 클라이언트의 사용에 따라 반환 타입의 하위 타입이면 선택하여 반환을 할 수 있다.


### 5) 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.


## 정적 팩터리 메서드 단점

### 1) 상속을 하려면 public 이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

정적 팩터리 메서드는 static 메서드이기 때문에 상속이 불가능하다. 마찬가지로 컬렉션 프레임워크의 유틸리티 구현 클래스들은 상속할 수 없다.

### 2) 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

생성자처럼 명확히 드러나지 않아 API 문서나 메서드명이 명확하지 않다면 찾기도 어렵고 사용법도 명확히 파악하기 어렵다.

- 정적 팩터리 메서드의 흔한 명명 방식들
    - **from** : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
        - ex) Date d = Date.from(instant);
    - **of** : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
        - ex) Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
    - **valueOf** : from과 of의 더 자세한 버전
        - ex) BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
    - **instance** 혹은 **getInstance** : (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
        - ex) StackWalker luke = StacWalker.getInstance(options);
    - **create** 혹은 **newInstance** : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
        - ex) Object newArray = Array.newInstance(classObject, arrayLen);
    - **getType** : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체의 타입이다.
        - ex) FileStore fs = Files.getFileStore(path)
    - **newType** : mewInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체의 타입이다.
        - ex) BufferedReader br = Files.newBufferedReader(path);
    - **type** : getType과 newType의 간결한 버전
        - ex) List<Complaint> litany = Collections.list(legacyLitany);

## 핵심 정리

정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는 것이 좋다.


- 참고 : 이펙티브 자바 3/E

