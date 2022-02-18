# 프로그램 에러

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이러한 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다.

이를 발생시점에 따라 `컴파일 에러(compile-time error)`와 `런타임 에러(runtime-error)`로 나눌 수 있다.

- **컴파일 에러** : 컴파일 시에 발생하는 에러
- **런타임 에러** : 실행 시에 발생하는 에러
- **논리적 에러** : 실행은 되지만, 의도와 다르게 동작하는 것

>
논리적 에러는 일반적으로 자바에 등록된 예외 케이스에 해당하지 않고 프로그래머에 의한 약속된 예외 상황에 따라 예외 재정의가 필요하다.

# 예외 처리(Exception Handling)

## 예외 클래스의 계층 구조

![](https://images.velog.io/images/host92/post/59024189-963d-48bd-a535-dfd2e73eb979/image.png)

- 출처 : https://rollbar.com/blog/java-exceptions-hierarchy-explained/

자바에서는 실행 시 발생할 수 있는 에러(Exception과 Error)를 클래스로 정의하여 제공한다. Throwable는 에러를 처리하는 최상위 클래스이고 그 다음 하위 클래스로 Exception과 Error 클래스가 정의된다. 

## Exception과 Error의 차이?

### Error 

JVM이 자원 부족하여 **메모리 부족(OutOfMemoryError)**나 **스택오버플로우(StackOverFlowError)** 등 **시스템 수준의 에러 상황이 발생하여 더 이상 수행을 계속할 수 없는 상황**을 나타낼 때 사용.
- **복구 가능성** : 프로그램 동작 내에서 복구 불가
- **발생 예시** : OutOfMemoryError, StackOverFlowError, IOError
- **패키지** : java.lang.Error
- **발생 상황** : 런타임 과정에서 발생

#### Error 예시

```java
public static void print(String myString) {
    print(myString);
}
```
위 코드에서는 print 메서드가 무한으로 재귀 호출되어 처리되지 않는 오류를 발생시키고 있다. print 메서드를 호출하면 프로그램 동작 중에 아래의 메시지를 출력하고 예상치 못한 종료가 된다.

```java
Exception in thread "main" java.lang.StackOverflowError
at StackOverflowErrorExample.print(StackOverflowErrorExample.java:6)
```

>
- `StackOverflowError` 발생
예시에서 print 메서드가 재귀로 무한 반복 호출 될 때(오류 상황), Java 스레드 스택의 최대 크기에 도달할 경우 `StackOverflowError`가 발생하고 프로그램이 종료된다.(복구 불가능)

### Exception 

프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

- **복구 가능성** : 프로그램 동작 전에 수정 및 예외 상황을 대비하여 복구 가능
- **발생 예시** : ClassNotFoundException, NullPointerException, ClassCastException
- **패키지** : java.lang.Exception
- **발생 상황** : 런타임 과정 또는 컴파일 과정에서 발생

**Exception은 Exception(=Checked)와 Runtime Exception(=Unchecked)로 나눠진다.**

## RuntimeException과 그 외 Exception의 차이

Exception 클래스의 하위 클래스는 크게 Runtime Exception과 그 외 다른 Exception들로 구분할 수 있다.

### RuntimeException

Runtime Exception은 프로그래머가 개발한 코드가 실행중에 언제든지 발생할 수 있는 예외이다. 이는 프로그래머가 코드상에 예상치 못한 실수를 하여 발생하는 예외이기 때문에 예측이 불가능하여 최대한 주의하여 개발해야 한다(컴파일러가 예외처리를 확인하지 않는다.).

#### RuntimeException NullPointerException 예시 

```java
public void writeToFile() {
try (BufferedWriter bw = null) {
        bw.write("Test");
    } catch (IOException ioe) {
        ioe.printStackTrace();
    }
}
```

```
Exception in thread "main" java.lang.NullPointerException
    at IOExceptionExample.writeToFile(IOExceptionExample.java:10)
    at IOExceptionExample.main(IOExceptionExample.java:17)
```

### 그 외의 Exception들

Exception은 컴파일 과정에서 발생할 수 있는 예외로 코드 상에 명시적으로 예외 처리(try-catch, throw...)를 해줘야 한다.

#### Exception 예시

```java
public void writeToFile() {
try (BufferedWriter bw = new BufferedWriter(new FileWriter("myFile.txt"))) {
        bw.write("Test");
    } catch (IOException ioe) {
        ioe.printStackTrace();
    }
}
```

위의 예시처럼 File IO처럼 반드시 예외 처리가 필요한 api는 try-catch문 또는 throw를 명시하지 않으면 컴파일 과정에서 에러가 발생한다.


## 예외 처리 방법 (try, catch, throw, throws, finally)



위에서 설명한 Exception 상황에 대해 대처하기 위해 자바에서는 try, catch, throw, throws, finally 과 같은 키워드를 사용하여 예외 처리한다.

### try-catch 문

try-catch 문은 예외가 발생가능성 있는 api를 사용하는 위치를 try 블럭안에 작성하고 호출한 api에서 예외가 발생했을 때 대처를 어떻게 할지 catch 블럭에 명시하여 예외 발생을 처리하는 방법이다.

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (Exception2 e2) {
    // Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (ExceptionN eN) {
    // ExceptionN가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

> JDK1.7부터 catch 블럭에 '|'을 사용하여 다중 catch 블럭을 합칠 수 있다.
```java
} cactch (Exception1 | Exception2 e) {
    // Exception1 또는 2 발생
}
```
해당 방법은 방법이 있다는 것만 알고 넘어가면 될 것 같다. 왜냐면 내부에서 개별 예외처리를 결국 해줘야 하기 때문이다.(그렇지 않다면 두 예외의 공통된 더 상위 클래스로 처리하면 된다.)

위 사용 방법 정의에서 실행흐름은 try 블럭 내에서 예외가 발생하지 않으면 catch 블럭을 거치지 않고 다음 동작을 진행한다. 만약 try 블럭 내에서 예외가 발생한다면 발생한 예외에 일치하는 catch 동작이 진행된다. 그 다음엔 바로 블럭을 빠져나가 다음 동작이 진행된다.

> 
다중 예외처리를 할 때는 예외마다 처리를 하려면 가장 먼저 나오는 catch 블럭의 Exception이 가장 하위 클래스이어야 한다.
```java
} catch (Exception e) {  // 예외 클래스중 가장 부모 클래스
    // Exception 발생 처리
} catch (IOException e) {  // <- 컴파일 에러 발생
    // IOException 발생 처리, 이미 위에서 Exception 처리를 모두 해서 컴파일 에러
}
```
catch 블럭 내부에 try-catch문을 또 선언했을 때 역시 마찬가지로 더 안에 있는 catch 블럭에 밖의 catch 블럭 예외 클래스보다 상위 예외 클래스를 선언해야 한다.



### try-catch-finally 문

finally 블럭은 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시키는데 사용된다.

```java
try { 
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // 예외처리를 위한 문장을 적는다.
} finally {
    // 예외의 발생여부에 관계없이 항상 수행되어야 하는 문장들을 넣는다.
    // finally 블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```

finally 블럭은 try 블럭이 실행되거나 catch 블럭이 실행되거나 혹은 try, catch 블럭 중간에 return 이 호출되어도 무조건 finally 블럭은 처리가 된다.

> **finally 블럭에서 절대 return 하면 안된다.**
finally 블럭에서 return하면 try 블록 안에서 발생한 예외는 무시되고 finally 블럭을 거쳐서 정상 종료가 된다.(예외 확인 불가능)

### try-catch-resources 문

JDK1.7부터 지원되는 기능으로 입출력과 같은 자원이 필요한 클래스들은 사용이 후 꼭 사용했던 자원을 반환해줘야 하는데 이를 좀 더 쉽게 처리해준다.

```java
try (사용 후 반환할 자원 선언) {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // 예외처리를 위한 문장을 적는다.
} 
```

#### try-catch-resources 사용 예시

```java
static String firstLineOfFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(
            new FileReader(path))) {
        return br.readLine();
    }
}
```


### 예외 발생시키기(throw)와 예외 던지기(exception re-throwing)

프로그래머가 예상치 않은 프로그램 동작에 대해 고의로 예외를 발생시키는 방법이 있다.

예외를 발생시킬 때는 **new를 통해 예외를 만들고 throw로 만든 예외를 발생**시킨다.

```java
Exception e = new Exception("고의적인 예외")  // 예외 객체 생성
throw e;  // 고의로 만든 예외 발생
```

또한 throw 키워드는 메서드에 선언하여 메서드에서 발생한 예외를 처리하지 않았을 때, 예외를 호출한 곳으로 떠넘기는 역할을 한다.

```java
void method1() throws Exception1, Exception2, ... ExceptionN {
    // 메서드의 내용
}
```

> throws는 메서드 선언 때 사용되고 여러개 Exception 던질 때 ','로 구분한다.
throw는 사용자가 고의로 예외를 발생시킬 때도 사용이 가능하다.

#### 예외 발생과 던지기 예시

```java
public class ExceptionThrowEx {
    public static void main(String[] args) {
        try {
            method1();  // 5. method1에서 던져진 예외를 확인한다.
        } catch (Exception e) {
            // 6. method1에서 던져진 예외를 처리한다.
            System.out.println("main 메서드에서 예외가 처리되었습니다.");
        }
    }

    public static void method1() throws Exception {  // 4. 예외를 호출한 곳으로 던진다.
        try {
            throw new Exception();  // 1. 예외를 만든다.
        } catch (Exception e) {
            // 2. try 블럭에서 만든 예외 처리를 한다.
            System.out.println("method1 메서드에서 예외가 처리되었습니다.");
            throw e;    // 3. 다시 예외를 발생시킨다.
        }
    }
}
```

```
method1 메서드에서 예외가 처리되었습니다.
main 메서드에서 예외가 처리되었습니다.
```

위 예시를 통해 예외를 발생시키고 던지는 방법을 설명한다. method1 메서드에서 throw new를 통해 Exception을 생성하는 동시에 method1()을 호출하는 곳으로 예외를 던진다.


### 연결된 예외(chained exception)

한 예외가 다른 예외를 발생시킬 수 있다. 예를 들어 예외 A가 예외 B를 발생시켰다면, A를 B의 '원인 예외(cause exception)'라고 한다.

#### chained exception 예시

```java
public class ExceptionChainedEx {
    public static void main(String[] args) {
        try {
            method1();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void method1() throws Exception {
        String findClassName = "ChainedClass";
        try {
            Class.forName(findClassName);
        } catch (Exception e) {
            // ClassCastException를 원인 예외로 지정
            ClassCastException exception = new ClassCastException(findClassName);
            exception.initCause(e); // 지정한 예외를 원인으로 설정
            throw exception;
        }
    }
}
```

### 사용자 정의 예외

기존에 정의된 예외 클래스 외에 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다.
보통 Exception 클래스 또는 RuntimeException 클래스를 상속받아서 만든다.

#### 사용자 정의 예외 예시

아래 예시는 로그인 시 발생하는 예외처리를 사용자 정의 예외 클래스로 한 예시이다.

```java
public class LoginEx {
    public static void main(String[] args) {
        try {
            login("white", "12345");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }

        try {
            login("blue", "54321");
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }

    public static void login(String id, String password) 
        throws NotExistIDException, WrongPasswordException {

        if (!id.equals("blue")) {
            throw new NotExistIDException("아이디가 존재하지 않습니다.");
        }

        if (!password.equals("12345")) {
            throw new WrongPasswordException("패스워드가 틀립니다.");
        }
    }
}

class WrongPasswordException extends Exception {
    public WrongPasswordException() {}
    public WrongPasswordException(String message) {
        super(message);
    }
}

class NotExistIDException extends Exception {
    public NotExistIDException() {}
    public NotExistIDException(String message) {
        super(message);
    }
}


```


-------------------------


이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
해당 글을 참고하시거나 퍼가실 때는 출처 링크 부탁드립니다 :)

참고
1. [백기선-자바스터디](https://github.com/whiteship/live-study/)
2. 이것이 자바다 - 신용권의 Java 프로그래밍 정복
3. 자바의 정석 책 3rd Edition
4. [Oracle-docs: Class Throwable](https://docs.oracle.com/javase/8/docs/api/java/lang/Throwable.html)
5. [Java Exceptions Hierarchy Explained](https://rollbar.com/blog/java-exceptions-hierarchy-explained/)
6. [예외처리](https://www.notion.so/3565a9689f714638af34125cbb8abbe8)
7. [이펙티브-자바-item09](https://github.com/JJungwoo/tech-note/blob/main/book/effective-java/chapter02/item09.md)

