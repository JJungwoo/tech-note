# 정확한 답이 필요하다면 float와 double은 피하라

### float and double 타입 문제점

float과 double 타입은 과학과 공학 계산용으로 설계되었다. 이진 부동 소수점 연산에 쓰이며, 넓은 범위의 수를 빠르게 정밀한 **'근사치'**로 계산하도록 설계되어 있다. 
따라서 **금융 관련 계산과 같은 정확한 결과가 필요할 때는 float과 double 타입은 맞지 않는다.** 0.1 혹은 10의 음의 거듭제곱 수(10^-1, 10^-2 등)를 표현할 수 없기 때문이다.

간단한 예를 통해 소수점 연산 결과에 오차가 있음을 확인할 수 있다.

```java
System.out.println(1.03 - 0.42);
```

위 코드의 결과는 0.6100000000000001 이다. 이처럼 소수점 결과에서 약간의 오차가 있음을 확인할 수 있다.

만약 금융 계산에 부동소수 타입을 사용했다면 아래의 예시처럼 조금의 오차를 확인할 수 있다.
1 달러가 있을 때, 10센트씩 물건을 산다면 얼마나 남는가에 대해 아래 코드로 표현하였다.

```java
public static void main(String[] args) {
    double funds = 1.00;
    int itemsBought = 0;
    for (double price = 0.10; funds >= price; price += 0.10) {
        funds -= price;
        itemsBought++;
    }
    System.out.println(itemBought + "개 구입");
    System.out.println("잔돈(달러):" + funds);
}
```

위 예시 코드 연산의 결과는 3개 구입과 잔돈은 0.3999999999999999 이다.
이처럼 1.00 값에서 0.10씩 뺏을 때 남은 연산 결과의 소수 값이 잘못되었음을 확인할 수 있다.

따라서 **금융 계산 같이 정확한 연산이 필요한 기능에는 BigDecimal, int 혹은 long을 사용해야 한다.**

### BigDecimal 타입 방식

```java
public static void main(String[] args) {
    final BigDecimal TEN_CENTS = new BigDecimal(".10");

    int itemsBought = 0;
    BigDecimal funds = new BigDecimal("1.00");
    for (BigDecimal price = TEN_CENTS;
            funds.compareTo(price) >= 0;
            price = price.add(TEN_CENTS)) {
        funds = funds.subtract(price);
        itemsBought++;
    }
    System.out.println(itemsBought + "개 구입");
    System.out.println("잔돈(달러):" + funds);
}
```

위 예시 코드 연산의 결과는 4개 구입과 잔돈 0.00 이다. 올바른 답이 나온것을 확인할 수 있다.

하지만 BigDecimal에는 단점이 두 가지 있다.
1. 기본 타입보다 쓰기가 훨씬 불편하다.
2. 훨씬 느리다.

### int or long 타입 방식

BigDecimal 대신에 int, long 타입을 쓸 수도 있다.

대신 그 경우에는 **다룰 수 있는 값의 크기가 제한되고 소수점 관리를 직접해야 한다.**

```java
public static void main(String[] args) {
    int funds = 100;
    int itemsBought = 0;
    for (int price = 10; funds >= price; price += 10) {
        funds -= price;
        itemsBought++;
    }
    System.out.println(itemsBought + "개 구입");
    System.out.println("잔돈(달러):" + funds);
}
```

위 예시 코드는 달러 대신에 센트 단위로 연산을 수행하여 위의 소수점 연산 문제를 해결한 방식이다.

## 핵심 정리

- 정확한 답이 필요한 계산에는 float이나 double을 피하라.
- 코딩 시의 불편함이나 성능 저하를 신경 쓰지 않겠다면 BigDecimal을 사용하라.
  - BigDecimal에서 반올림 제어 기능을 제공한다.
- 성능이 중요하고 소수점을 직접 추적할 수 있고 숫자가 너무 크지 않다면 int나 long을 사용하라.
  - int : 아홉 자리 십진수
  - long : 열여덟 자리 십진수
  - BigDecimal : 열여덟 자리를 넘어간 십진수

- 참고 : 이펙티브 자바 3/E
