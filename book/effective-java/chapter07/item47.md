# 반환 타입으로는 스트림보다 컬렉션이 낫다

- 자바 메서드의 원소 시퀀스 변환 타입
  - Collection Interface(Collection, Set, List)
  - Iterable
  - Array
  - Stream (JDK 8)

### 다양한 변환 타입중 어떤걸 사용해야 할까??

- **Collection 인터페이스** : 기본으로 제공하는 반환 타입
- **Iterable 인터페이스** : for-each 문에서만 쓰이거나 반환된 원소 시퀀스가 일부 Collection 메서드를 구현할 수 없을 때
- **배열(Array)** : 반환 원소들이 기본 타입이거나 성능에 민감한 상황
- **Stream** : JDK 1.8 부터 지원하는 기능

-> JDK 8부터 제공되는 **스트림(Stream)**은 반복(iteration)을 지원하지 않는다. 따라서 스트림과 반복을 알맞게 조합해야 한다.

### 스트림을 반복자로 우회하여 사용할 수는 있다.

- 직설적인 형변환 방법
```java
for (ProcessHandle ph : (Iterable<ProcessHandle>) 
                        ProcessHandle.allProcesses().iterator()) {
  // 프로세스 처리
}
```

- 어댑터 메서드를 활용한 형변환 방법
```java
public static <E> Iterable<E> iterableOf(Stream<E> stream) {
	return stream::iterator;
}
```
```java
for (ProcessHandle ph : iterableOf(ProcessHandle.allProcesses()) {
  // 프로세스 처리
}
```

### Iterable<E>을 Stream<E>로 중개하는 어댑터

반대로 Iterable을 Stream 으로 타입 변환해서 사용자에게 제공할 수도 있다.

```java
public static<E> Stream<E> streamOf(Iterable<E> iterable) {
    return StreamSupport.stream(iterable.spliterator(), false);
}
```

**공개 API를 작성할 때는 스트림 파이프라인을 사용하는 사람과 반복문을 사용하는 사람 모두를 배려하여 작성해야 한다.**
-> 보통 Collection 인터페이스는 Iterable의 하위 타입이면서 stream 메서드도 제공하기 때문에 위 조건을 모두 지원한다.<br>
그렇기 때문에 **원소 시퀀스를 반환하는 공개 API의 반환 타입에는 Collection이나 그 하위 타입을 쓰는게 일반적으로 최선이다.**

### 컬렉션 내의 반환할 시퀀스가 크다면 전용 컬렉션을 구현해라

컬렉션을 반환 타입으로 할 때 반환하는 시퀀스의 크기가 메모리에 올려도 안전할 만큼 작지 않고 너무 클때는 전용 컬렉션을 구현하는 방안을 검토해봐야 한다.

- ex) 특정 집합에서의 멱집합을 구하는 예시에서 각 원소 인덱스를 비트 벡터로 사용하여 전용 컬렉션을 구현할 수 있다.

```java
public class PowerSet {
    public static final <E> Collection<Set<E>> of(Set<E> s) {
       List<E> src = new ArrayList<>(s);
       if(src.size() > 30) {
           throw new IllegalArgumentException(
           "집합에 원소가 너무 많습니다(최대 30개).: " + s);
       }

       return new AbstractList<Set<E>>() {
           @Override
           public int size() {
               return 1 << src.size();
           }

           @Override
           public boolean contains(Object o) {
               return o instanceof Set && src.containsAll((Set) o);
           }

           @Override
           public Set<E> get(int index) {
               Set<E> result = new HashSet<>();
               for (int i = 0; index != 0; i++, index >>=1) {
                   if((index & 1) == 1) {
                       result.add(src.get(i));
                   }
               }
               return result;
           }
       };
    }
}
```

>멱집합: 한 집합의 모든 부분집합을 원소로 하는 집합


# 핵심 정리

- 원소 시퀀스를 반환하는 메서드를 작성할 때는 되도록 스트림과 반복 모두 처리 가능하게 제공하자
- 더 좋은 방법은 컬렉션을 반환하는 방법이고 컬렉션을 반환할 때는 보통 표준 컬렉션에 담아 반환하고 너무 크기가 크면 전용 컬렉션을 구현해야 한다
- 모두를 제공할 수 없고 컬렉션을 반환하는 것이 불가능하다면 Stream을 선택하는게 좋다, 이후 지원하게 업데이트 될 수 있으니!(물론 상황에 따라 다르다)

