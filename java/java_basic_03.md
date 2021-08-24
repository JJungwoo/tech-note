
# 자바 연산자

자바에서는 아래의 연산자들을 제공한다.

|종류|연산자|설명|
|---|---|---|
|산술 연산자| + - * / % << >> |사칙 연산과 나머지 연산(%)|
|비교 연산자| > < >= <= == != | 크고 작음과 같고 다름을 비교|
|논리 연산자| && \|\| ! & \| ^ ~ |'그리고(AND)'와 '또는(OR)'으로 조건을 연결|
|대입 연산자|        =       |우변의 값을 좌변에 저장|
|기타| (type) ?: instanceof|형변환 연산자, 삼항 연산자, instanceof 연산자|


> 일반적으로 산술 연산자는 기본형(primitive)에만 사용이 가능하다. 따라서 비교연산자를 통해 문자열을 비교할 때는 `==` 연산을 사용하면 안되고 String 객체의 equals() 메서드를 사용해야 한다. 이처럼 기본형이 아닌 참조형(Reference)은 별도의 메서드를 통해 비교를 해야한다.

> 
산술 연산을 할 때 주의할 점으로 만약 **부동 소수점 연산에 정밀한 값을 계산하려면 float과 double 타입은 사용하지 않는 것이 좋다.**
그에 따른 해결 방법으로 아래의 링크를 참고한다.[3]
>
[정확한 답이 필요하다면 float와 double은 피하라](https://github.com/JJungwoo/tech-note/blob/main/book/effective-java/chapter09/item60.md)

## 산술 연산자에서 형변환 예시

기본 타입에서 자신의 범위를 벗어나면 overflow가 발생하여 예상한 값을 벗어난 결과가 발생한다. 이는 자신의 타입의 범위를 벗어나게되어 최대 크기를 넘어 다시 범위의 가장 아래(음수의 끝)부터 연산이 되기 때문이다.

```java
long a = 1_000_000 * 1_00_000; // a = -727379968
long b = (long)1_000_000 * 1_000_000; // b = 1000000000000
(b == 1_000_000L * 1_000_000);
(b == 1_000_000 * 1_000_000L);
```

위에서 언급한 것 처럼 둘 중에 하나만 형변환하려는 타입으로 바꿔주면 된다.
산술 과정에서 순서는 산술 결과(5 / 2 = 2.5) 이후 그 결과(2.5)에 대한 형변환이 이뤄진다.

> 산술 변환이란? 연산 수행 직전에 발생하는 피연산자의 자동 형변환
1) 두 피연산자의 타입을 같게 일치시킨다.(보다 큰 타입으로 일치)
```java
long + int -> long + long -> long
float + int -> float + float -> float
double + float -> double + double -> double
```
ex) 5 / (float)2 -> 5.0f / 2.0f -> 2.5f
2) 피연산자의 타입이 int 보다 작은 타입이면 int로 변환된다.
```java
byte + short -> int + int -> int
char + short -> int + int -> int
```


## instanceof 연산자

instanceof 연산자는 자바에서 상속 개념에 사용되는 연산자로 특정 객체에 대한 실제 클래스 타입(종속된 클래스 포함)를 확인할 때 사용하는 연산자이다. 보통 조건문에 사용되어 형변환이 가능한지 체크하는 용도로 쓰인다.

```java
class A {
    ...
}

class B extends A {
    ....
}

class C extends A {
    ...
}

B b = new B();
b instanceof B == true;
b instanceof A == true;
b instanceof C == false;
```



## 이름 붙은 반복문

break 문은 근접한 단 하나의 반복문만 벗어날 수 있기 때문에, 여러 개의 반복문이 중첩된 경우에는 break 문으로 중첩 반복문을 완전히 벗어날 수 없다. 이때는 중첩 반복문 앞에 이름을 붙이고 break 문과 continue 문에 이름을 지정해 줌으로써 하나 이상의 반복문을 벗어나거나 건너뛸 수 있다.

```java
loop1 : for(int i=1;i<10;i++) {
    for(int j=i;j<10;j++) {
        if (j == 5) {
            // loop1, 즉 밖의 for문이 break 되어 전체 루프가 종료된다.
            break loop1; 
        }
    }
}

```

## swtich 추가 기능

java 12버전에 처음 등장한 switch expression으로 switch문에서 추가 표현식이 가능해졌다. 그동안 동일한 case를 표현할 때 하나하나 case를 넣었는데, 더이상 그렇게 하지않고 아래 예시처럼 `,`를 사용하여 동일한 case를 표현할 수 있게 되었다. 또한 switch문에서 결과를 리턴으로 받을 수도 있다.[2]

사용 예시 : [프로그래머스 문제 풀이 코드에서 활용한 예시](https://github.com/JJungwoo/algorithm/blob/master/Java/programmers/%EC%9C%84%ED%81%B4%EB%A6%AC_%EC%B1%8C%EB%A6%B0%EC%A7%80_2%EC%A3%BC%EC%B0%A8.java)


```java
private static String getNumberViaBreak(int number) {
      // 결과 리턴
      String result = switch (number) {
          case 1, 2:
              break "one or two";
          case 3:
              break "three";
          case 4, 5, 6:
              break "four or five or six";
          default:
              break "unknown";
      };
      return result;
}
```

그리고 13버전에서 `->` 표현식을 통해 `:`를 대신할 수 있다. 

```java
private static String getNumberViaCaseL(int number) {
    return switch (number) {
        case 1, 2 -> "one or two";
        case 3 -> "three";
        case 4, 5, 6 -> "four or five or six";
        default -> "unknown";
    };
}
```

마지막으로 java 14버전에서는 break를 yield로 결과를 리턴하게 변경하였다.

```java
private static String getNumberViaCaseL2(int number) {
    return switch (number) {
        case 1, 2 -> "one or two";
        case 3 -> "three";
        case 4, 5, 6 -> {
            int i = 0;
            i++;
            yield "four or five or six :" + 1;
        }
        default -> "unknown";
    };
}
```

## 람다식(Lambda expression)

람다식은 java 8버전부터 추가되어 자바 언어가 객체지향언어인 동시에 함수형 언어로 사용될 수 있게되었다. 
람다식이란 메서드를 하나의 '식(expression)'으로 표현한 것이다. 람다식은 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다.
메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 '익명함수(anonymous function)'이라고도 한다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random() * 5) + 1);
```

위의 람다식을 메서드로 표현하면 아래와 같다.

```java
int method() {
    return (int)(Math.random() * 5) + 1;
}
```

람다식에 대한 특징
1) 간결하고 이해하기 쉽다.
2) 메서드를 따로 선언하지 않고 바로 사용이 가능하다.
3) 메서드의 모든 기능을 그대로하여 메서드를 변수처럼 다룰수 있게 되었다.

> 
- 메서드와 함수
**메서드** : 클래스 내부의 기능 -> 메서드
**함수** : 람다식을 통해 하나의 독립적인 기능 -> 함수
= 결국 둘다 함수라고 볼 수 있지만 혼동되지 않게 위치에 따라 용어를 구분한다.

### 람다식 작성하기

람다식은 '익명 함수' 답게 메서드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통{} 사이에 '->'를 추가한다.

- 메서드
반환타입 메서드이름 (매개변수 선언) {
&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp문장들
}

- 람다식
~~반환타입 메서드이름~~ (매개변수 선언) -> {
&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp문장들
}

#### 람다식 예시 코드

- 메서드
```java
int max(int a, int b) {
    return a > b ? a : b;
}
```

- 람다식
```java
(int a, int b) -> a > b ? a : b;
OR 람다식은 타입 추론이 가능한 경우 모두 생략가능하다.
(a, b) -> a > b ? a : b;
```

# 내용 정리

![](https://images.velog.io/images/host92/post/1b014323-5363-4bcc-945b-ad0c86651685/java-mindmap03.png)

----------------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
전체적인 글은 자바의 정석 책을 참고하였기 때문에 글귀에 참고 표기를 넣지 않았습니다.

참고
1. [백기선-자바스터디](https://github.com/whiteship/live-study)
2. [What is new in Java 13](https://mkyong.com/java/what-is-new-in-java-13/)
3. 이펙티브 자바 3rd Edition
4. 자바의 정석 책 3rd Edition
