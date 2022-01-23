# 스트림 병렬화는 주의해서 적용하라

동시성 프로그래밍을 할 때는 **안전성(safety)** 과 **응답 가능(liveness)** 상태를 유지해야 한다. 스트림에서 제공하는 병렬처리 역시 마찬가지이다.

- 소수를 구하는 코드 예시

```java
public static void main(String[] args) {
    primes().map(p -> TWO.pow(p.intValueExact()).subtract(ONE))
            //.parallel() // 스트림 병렬 파이프라인 메서드
            .filter(mersenne -> mersenne.isProbablePrime(50))
            .limit(20)
            .forEach(System.out::println);
}

public static Stream<BigInteger> primes() {
    return Stream.iterate(TWO, BigInteger::nextProbablePrime);
}
```

위의 소수를 구하는 코드에서 좀 더 빠르게 결과를 받기 위해 스트림에서 제공하는 parallel() 메서드를 활용한다면 
병렬 처리로 성능이 빨라질 것이라 생각하지만 그렇지 않다. 오히려 **응답 불가(liveness)** 상태가 되어 **CPU를 90% 이상 사용하며 무한 루프에 빠지게 된다.**

### Stream.iterate 거나 중간 연산으로 limit를 쓰면 파이프라인 병렬화로는 성능 개선을 기대할 수 없다.

1. 스트림 라이브러리(Stream.iterate)가 이 파이프라인을 병렬화하는 방법을 찾아내지 못했기 때문이다.<br>
-> 결국 해당 스트림 라이브러리는 병렬처리를 지원하지 않는다는 말 같다..
2. limit를 사용할 때 CPU 코어가 남는다면 원소를 몇 개 더 처리한 후 제한된 개수 이후의 결과를 버려도 상관없다고 가정한다.<br>
-> 즉 병렬처리하는 연산들은 처리속도가 비슷해야 성능이 좋다 하지만 소수를 계산하는 과정은 찾을 때마다 이전 연산의 두배가 걸리기 때문에 문제가 된다.

### 스트림 병렬 메서드의 좋은 활용 예시

조건이 잘 갖춰지면 parallel 메서드 호출 하나로 거의 프로세서 코어 수에 비례하는 성능 향상을 확인 할 수 있다.<br>
아래의 코드는 **parallel 메서드 호출 사용 시 전보다 3.37배가 더 빠르다(쿼드코어PC).**

```java
static long pi(long n) {
	return LongStream.rangeClosed(2, n)
		.parallel()
		.mapToObj(BigInteger::valueOf)
		.filter(i -> i.isProbablePrime(50))
		.count();
}
```

### 병렬화 효율이 좋은 자료구조

대체로 스트림의 소스가 ArrayList, HashMap, HashSet, ConcurrentHashMap의 인스턴스거나 배열, int 범위, long 범위일 때 
병렬화의 효과가 가장 좋다.

>이유<br>
>1. 해당 자료구조들은 모두 데이터를 원하는 크기로 정확하게 손쉽게 나눌 수 있어서 일을 다수의 스레드에 분배하기에 좋다<br>
>2. 해당 원소들은 순차적으로 실행할 때의 참조 지역성(locality of reference)이 뛰어나다는 것이다(원소들이 메모리에 연속해서 저장되기 때문)

### 스트림 파이프라인 병렬 수행 효율에 영향을 주는 메서드

- 스트림에서 제공하는 종단 연산 중 병렬화에 가장 적합한 것은 축소(reduction), reduce 메서드 중 하나 또는 min, max, count, sum 이다.
- anyMatch, allMatch, noneMatch 처럼 조건에 맞으면 바로 반환되는 메서드도 병렬화에 적합하다. 

>반면, 가변 축소를 수행하는 Stream의 collect 메서드는 병렬화에 적합하지 않다.
(컬렉션들을 합치는 부담이 크기 때문)

# 핵심 정리

- 계산도 올바로 수행하고 성능도 빨라질 거라는 확신 없이는 스트림 파이프라인 병렬화는 시도조차 하지 마라
- 스트림을 잘못 병렬화하면 프로그램을 오작동하게 하거나 성능을 급격히 떨어뜨린다
- 병렬화로 수정한 이후에 성능을 따져보고 그 결과가 정확하고 성능도 좋아졌다고 확신했을 때 적용해라
