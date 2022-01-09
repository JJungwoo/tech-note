# 배열보다는 리스트를 사용하라

배열과 제네릭 타입의 중요한 두 가지 차이점
- 배열은 **공변**이고 제네릭은 **불공변**이다
>공변은 상위타입과 하위타입에 대해 Sub가 Super의 하위 타입이라면 Sub[]도 Super[]의 하위 타입이 된다(즉, 함께 변한다).
- 배열은 런타임 시에 **타입이 실체화**되고 제네릭은 런타임 시 **타입 정보가 소거**된다.
>타입이 실체화되는 것은 런타임 과정에서 원소에 대한 타입을 인지하는 것을 말함, 따라서 타입이 소거되는 것은 런타임 과정에서 타입을 인지할 수 없음을 말한다.

- 배열(런타임에 실패) 
```java
Object[] objectArray = new Long[1];
// 배열은 공변이기 때문에 Object(상위 타입)에 String(하위 타입)을 할당에 문제가 없다
// 하지만 실제 값은 Long 타입이기 때문에 런타임시 에러가 발생한다.
objectArray[0] = "타입이 달라 넣을 수 없다.";
// 배열은 실체화되기 때문에 런타임 시 위 코드에서 ArrayStoreException가 발생한다.
```

- 리스트(컴파일에 실패)
```java
// 제네릭은 불공변이기 때문에 컴파일 시 타입이 변하지 못하여 컴파일 실패가 된다.
List<Object> objectList = new ArrayList<Long>(); // 호환되지 않는 타입이다.
objectList.add("타입이 달라 넣을 수 없다");
// 만약 컴파일 에러가 발생하지 않았다면 위 코드는 문제가 발생하지 않는다.
// 그 이유는 제네릭 타입은 런타임 과정에서 타입 정보가 소거되기 때문이다.
// 따라서 이후에 해당 타입을 잘못 형변환해서 사용하면 런타임 시 ClassCastException이 발생한다.
```

>제네릭은 런타임 과정에서 타입을 소거하기 때문에 런타임에서는 문제가 발생하지 않고 이후에 해당 원소를 잘못 사용할 위험이 있다.

### 배열로 형변환할 때 오류나 경고 발생 해결 방법과 예시

배열로 형변환할 때 제네릭 배열 생성 오류나 비검사 형변환 경고가 뜨는 경우 대부분은 배열인 E[] 대신 컬렉션인 List<E>를 사용하면 해결된다.
>코드가 조금 복잡해지고 성능이 살짝 나빠질 수 있지만 타입 안전성과 상호운용성은 좋아진다.

- 예시
```java
public class Chooser {
    private final T[] choiceArray;

    public Chooser(Collection<T> choices) {
        // 오류 발생 incompatible types: java.lang.Object[] cannot be converted to T[]
        this.choiceArray = choices.toArray();
    }
    
    // 이 메서드를 호출할 때마다 변환된 Object를 원하는 타입으로 형변환해야 한다.
    // 따라서 런타임 시 형변환 오류의 가능성이 있다.
    public Object choose() {
        Random rnd = ThreadLocalRandom.current();
        return choiceArray[rnd.nextInt(choiceArray.length)];
    }
}
```

- 컴파일 오류 메시지 출력 결과
```java
Chooser.java:9: error: incompatible types: Object[] cannot be
converted to T[]
        choiceArray = choices.toArray();
                                     ^
  where T is a type-variable:
    T extends Object declared in class Chooser
```

- 해결 방법
```java
// Object 배열을 T 배열로 형변환하면 된다.
this.choiceArray = (T[]) choices.toArray();
```

- 또다른 경고 메시지 출력 결과
```java
Chooser.java:9: warning: [unchecked] unchecked cast
        choiceArray = (T[]) choices.toArray();
                                           ^
  required: T[], found: Object[]
  where T is a type-variable:
T extends Object declared in class Chooser
```

해당 메시지는 T가 무슨 타입인지 알수 없으니 컴파일러는 런타임 시 안전을 보장할 수 없다는 메시지다.
>제네릭에서는 원소의 타입 정보가 소거되어 런타임에는 무슨 타입인지 알수 없다.


이때 해결 방법은 주석과 애너테이션을 통해 경고를 숨기거나(item27) 
배열 대신 리스트를 사용하여 비검사 형변환 경고를 제거할 수 있다.
<br>
  
  
- 배열 대신 리스트를 사용하여 경고 메시지 제거 예시
```java
class Chooser<T> {
    private final List<T> choiceList;

    public Chooser(Collection<T> choices) {
        this.choiceList = new ArrayList<>(choices);
    }

    public T choose() {
        Random rnd = ThreadLocalRandom.current();
        return choiceList.get(rnd.nextInt(choiceList.size()));
    }
}
```

배열을 리스트로 바꿔 런타임에 ClassCastException을 만날 일은 없다.

# 핵심 정리

- 배열과 제네릭에는 매우 다른 타입 규칙이 적용된다. 
- 배열은 공변이고 타입 정보 실체화 => 런타임에는 타입 안전, 컴파일 타임에는 x
- 제네릭은 불공변이고 타입 정보가 소거 => 런타임에는 x, 컴파일 타임에 타입 안전
- 컴파일 오류나 경고를 만나면 배열보단 리스트를 통해 해결하자


