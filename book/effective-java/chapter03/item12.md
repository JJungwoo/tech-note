# toString을 항상 재정의하라

일반적으로 Object의 toString 메서드는 **클래스_이름@16진수로_표시한_해시코드** 를 반환한다.

프로그래머는 toString의 일반 규약에 따르면 `'간결하면서 사람이 읽기 쉬운 형태의 유익한 정보'`를 반환해야 한다.

따라서 클래스의 용도에 따라 적절한 형태로 정보를 제공할 수 있게 toString 반환 형태를 재정의 해줘야 한다.

- 적절한 형태의 정보 제공 toString 재정의 예시
```java
X: {Jenny=PhoneNumber@adbbd}
O: {Jenny=707-867-5309}
```

또한 toString을 재정의 함으로서 println, printf, assert 등을 통해 디버깅하기 쉬운 장점이 있다.

포맷을 명시함으로서 향후 시스템이 망가지는 결과를 방지해야 한다.

- 포맷 명시 예시

```java
/**
* 전화번호의 문자열 표현을 반환하다.
* 문자열은 "XXX-YYY-ZZZZ" 형태의 12글자로 구성된다.
* XXX는 지역 코드, YYY는 프리픽스, ZZZZ는 가입자 번호다.
* 각각의 대문자는 10진수 숫자 하나를 나타낸다.
*
* 전화번호의 각 부분의 값이 너무 작아서 자릿수를 채울 수 없다면,
* 앞에서부터 0으로 채워나간다. 예컨대 가입자 번호가 123이라면
* 전화번호의 마지막 네 문자는 "0123"이 된다.
*/
@Override public String toString() {
  return String.format("%03d-%03d-%04d", areaCode, prefix, lineNum);
}
```

포맷을 명시하든 아니든 프로그래머의 의도는 명확히 밝혀야 한다.

toString이 반환한 값에 포함된 정보를 얻어올 수 있는 API를 제공하자.

- 포맷 명시하지 않은 예시

```java
/**
* 이 약물에 관한 대략적인 설명을 반환한다.
* 다음은 이 설명의 일반적인 형태이나,
* 상세 형식은 정해지지 않았으며 향후 변경될 수 있다.
* 
* "[약물 #9: 유형=사람, 냄새=테레빈유, 겉모습=먹물]"
*/
@Override public String toString() { ...}
```


## 핵심 정리

- 모든 구체 클래스에서 Object의 toString을 재정의하자.
- 상위 클래스에서 이미 알맞게 재정의한 경우는 예외다.
- toString을 재정의하여 유용한 정보를 읽기 좋은 형태로 제공하자.

