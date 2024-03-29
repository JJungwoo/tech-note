# 변수(variable)

변수란, 단 하나의 값을 저장할 수 있는 메모리 공간이다. 자바에서는 변수 타입을 **기본형(primitive type)**과 **참조형(reference type)**으로 구분할 수 있다.

- 변수 명명규칙
  - 대소문자가 구분되어 길이에 제한이 없다
    - True와 true는 서로 다른 것으로 간주된다.
  - 예약어를 사용해서는 안 된다.
    - true는 예약어라서 사용할 수 없지만, True는 가능하다.
  - 숫자로 시작해서는 안 된다.
    - top10은 허용하지만, 7up은 허용되지 않는다.
  - 특수문자는 '_'와 '$'만을 허용한다.
    - $harp은 허용되지만, S#arp은 허용되지 않는다.
    
- 변수명 작성 시 권장 규칙
  - 클래스 이름의 첫 글자는 항상 대문자로 한다.
    - 변수와 메서드의 이름의 첫 글자는 항상 소문자로 한다.
  - 여러 단어로 이루어진 이름은 단어의 첫 글자를 대문자로 한다.
    - lastIndexOf, Stringbuffer
  
  
- **클린 코드**[4]
  - 의도를 분명히 밝혀라
    - 변수명을 정할 때는 사용 의도를 드러나도록 지어야 한다.
  - 그릇된 정보를 피하라
    - 프로그래머에게 특수한 의미를 나타내는 단어를 오용하거나 흡사한 이름을 애매하게 사용하지 않는다.
  - 의미 있게 구분하라
    - 그저 단순히 복사를 할 때 a, b 라는 식으로 변수명을 짓지말고 source와 destination과 같이 의미 있는 이름을 사용한다.
    
    
>  ### 변수명 작성 시 일반적인 컨벤션
> - 변수에 소문자 [카멜표기법](https://ko.wikipedia.org/wiki/%EB%82%99%ED%83%80_%EB%8C%80%EB%AC%B8%EC%9E%90) 적용[1]
상수가 아닌 클래스의 멤버변수/지역변수/메서드 파라미터에는 소문자 카멜표기법을 사용한다.
>
**나쁜 예**
```java
private boolean Authorized;
private int AccessToken;
```
>
**좋은 예**
```java
private boolean authorized;
private int accessToken;
```


# 기본형(Primitive Type)

기본 타입 변수는 실제 값(data)을 저장하는 변수형 타입으로 다음과 같이 제공된다.
실제 값은 논리형, 문자형, 정수형, 실수형으로 계산을 위한 실제 값이 존재한다.

|---|1byte|2byte|4byte|8byte|
|---|---|---|---|---|
|정수형|byte|short|int|long|
|실수형|---|---|float|double|
|문자형|---|char|
|논리형|boolean|

> 
정수형의 기본 타입 : int
실수형의 기본 타입 : double
- 문자형 char 타입은 c언어와 다르게 유니코드(약 65,000개의 문자)를 지원하기 때문에 2byte까지 허용한다.[3]
- JDK 1.7부터 정수형 리터럴의 중간에 구분자 '_'를 넣을 수 있게 되어서 큰 숫자를 편하게 읽을 수 있다.
ex) long big = 100_000_000_000L;

# 참조형(Reference Type)

참조 타입 변수는 어떤 값이 저장되어 있는 주소(memory address)를 값을 저장하는 변수형 타입이다.

- 참조 타입 종류
  - 배열
  - 클래스
  - 열거형(enum)
  - 인터페이스

> 참조형 변수는 JVM에 따라 32bit -> 4byte, 64bit -> 8byte 크기를 갖는다.


## 형변환(캐스팅, casting)

형변환이란, 변수 또는 상수의 타입을 다른 타입으로 변환하는 것

- 형변환 예시

|변환|수식|결과|
|---|---|---|
|int -> char|(char)65|'A'|
|char -> int|(int)'A'|65|
|float -> int|(int)1.5f|1|
|int -> float|(float)1|10.0f|

- 자동 형변환

서로 다른 타입간의 대입이나 연산을 할 때, 형변환으로 타입을 일치시키는 것이 원칙이다. 그런데 경우에 따라 편의상 형변환을 생략해서 사용이 가능하다.(컴파일러가 생략한 형변환을 자동으로 추가해준다.)

> 자동 형변환의 규칙 : 기존의 값을 최대한 보존할 수 있는 타입으로 자동 형변환 한다.
```java
자동 형변환 가능 방향 : A->B (A에서 B로 자동 형변환 가능)
byte(1byte) -> short(2byte) -> int(4byte) -> long(8byte) -> float(4byte) -> double(8byte)
char(2byte) 타입은 int 타입으로 자동형변환 가능
```

- 형변환 주의 사항
  - boolean을 제외한 나머지 7개의 기본형은 서로 형변환이 가능하다.
  - 기본형과 참조형은 서로 형변환할 수 없다.
  - 서로 다른 타입의 변수간의 연산은 형변환을 하는 것이 원칙이지만, 값의 범위가 작은 타입에서 큰 타입으로의 형변환은 생략할 수 있다.



### 상수(Constant)

자바에 따로 상수형이라는 타입이 존재하지는 않는다. 따라서 보통 변하지 않는 값을 나타내는 변수를 사용할 때 접근제어자 **final** 를 통해 상수로 변수를 지정한다.`(enum, 열거형으로도 비슷한 표현이 가능하다.)` 일반적으로 상수 변수명은 모두 대문자로하고 단어 사이의 구분을 위해 언더스코어(_)를 사용한다. 상수는 선언과 동시에 초기화를 해서 사용해야 한다.
ex) PI, MAX_NUMBER

> 
**클린 코드** : 상수는 의미 있고 검색하기 쉬운 이름을 사용하라, 상수는 찾기도 어렵고 디버깅이 어렵기 때문에 최대한 길게 많이 쓰이지 않는 문자로 이름을 짓는 것이 좋다.[4]
**final** : 변수의 단 한번 초기화 이후 해당 변수는 값을 변경할 수 없는 상태가 된다.

### 리터럴 (Literal)

자바에서 리터럴은 **값(data)** 자체를 말한다. 앞서 언급한 변수나 상수에 할당이 되는 그 값 자체를 리터럴이라 한다.

- 문자열의 리터럴
  문자열과 다른 타입을 함께 사용하면 문자열 리터럴로 결합이 된다.

ex)
문자열 + **any type** -> 문자열 + **문자열** -> 문자열
**any type** + 문자열 -> **문자열** + 문자열 -> 문자열


>
**변수(variable)** : 하나의 값을 저장하기 위한 공간
**상수(constant)** : 값을 한번만 저장할 수 있는 공간
**리터럴(literal)** : 그 자체로 값을 의미하는 것

#### 접미사

리터럴 타입에도 타입이 존재한다. 정수형 숫자는 int 타입으로 인식하고 실수형 숫자는 double 타입으로 인식을 한다.

|종류|리터럴|접미사|
|---|---|---|
|논리형|false, true|없음|
|정수형|123, 0b010, 077, 0xFF, 100L|L|
|실수형|3.14, 3.0e8, 1.4f, 0x1.0p-1|f, d|
|문자형|'A', '1', '\n'|없음|
|문자열|"ABC", "123", "A", "true"|없음|

### 변수 스코프와 라이프타임

변수는 선언 위치에 따라 접근 가능한 범위가 존재한다. 이에 따라 변수의 라이프타임도 결정되어 사용할 수 있다.

- 변수 타입
  - 클래스 내부 변수
  - 블럭 내부 변수
  
#### 클래스 내부 변수

자바에서 클래스 내부에 선언되는 변수는 해당 클래스 내부에서 접근이 가능하고 유지된다.
아래의 예시에서 test 클래스 내부 value 변수의 스코프는 해당 클래스 내부 전역에서 사용 가능하다. 라이프 타임은 해당 클래스가 객체로서 할당되어 힙에 적재 되었을 때부터 해제 될 때까지이다.

```java
class test {
    int value;
    test(int value) {
    	this.value = value;
    }
    ...
}
```

#### 블록 내부 변수

블록 내부 변수는 특정 블록 내부에서 선언되어 유지되다 블록이 종료되는 순간 사라지게 된다.
아래의 블록 예시를 통해 스코프와 라이프타임을 설명한다.

- 일반 블록 및 반복문 블록
  - 블록 내부에서 선언했을 때, 블록 내부에서만 접근 및 사용이 가능하다.

```java
{
    int value;
}

while(true) {
    int value;
}

for(;;) {
    int value;
}
```

### var 타입

Java 10 버전부터 제공되는 변수형 타입으로 **var**가 존재한다. Java 11 버전부터는 람다에서 지원이 가능하다.[5,6]
c++에서의 auto 처럼 타입 추론이 가능하여 아래와 같이 활용할 수 있다.

```java
var list = new ArrayList<String>(); // infers ArrayList<String>
var stream = list.stream();         // infers Stream<String>
...
for (var person : personList) {
    // ...
}
```

-----------------------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
전체적인 글은 자바의 정석 책을 참고하였기 때문에 글귀에 참고 표기를 넣지 않았습니다.

- 참고
  1. [네이버컨벤션](https://naver.github.io/hackday-conventions-java/#var-lower-camelcase)
  2. [백기선-자바스터디](https://github.com/whiteship/live-study/issues/2)
  3. [\[C언어가 본\] Java(자바)의 자료형](https://blog.naver.com/chgy2131/222023121961)
  4. [클린 코드 2장 의미 있는 이름](https://github.com/JJungwoo/tech-note/blob/main/book/clean_code.md#2%EC%9E%A5_%EC%9D%98%EB%AF%B8_%EC%9E%88%EB%8A%94_%EC%9D%B4%EB%A6%84)
  5. [OpenJDK](https://openjdk.java.net/jeps/286)
  6. [Java 10에서 var 재대로 사용하기](https://dev.to/composite/java-10-var-3o67)
  7. 자바의 정석 책 3rd Edition
