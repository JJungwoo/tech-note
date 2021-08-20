# try-finally 보다는 try-with-resources를 사용하라

자바 라이브러리에는 close 메서드를 호출해 직접 닫아줘야 하는 자원이 많다.
InputStream, OutputStream, java.sql.Connection 등이 좋은 예다.

자원 닫기는 클라이언트가 놓치기 쉬워서 예측할 수 없는 성능 문제로 이어지기도 한다. 이런 자원 중 상당수가 안전망으로 finalizer를 활용하고 있지만 finalizer는 믿을만하지 못하다.

전통적으로 자원이 제대로 닫힘을 보장하는 수단으로 **try-finally**가 쓰였다.

```java
static String firstLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```

자원을 하나 더 사용한다면 더 복잡해져서 훌륭한 프로그래머조차 실수를 할 수 있다.

```java
static final int BUFFER_SIZE = 4096;
static void copy(String src, String dst) throws IOException {
    InputStream in = new FileInputStream(src);
    try {
        OutputStream out = new FileOutputStream(dst);
        try {
            byte[] buf = new byte[BUFFER_SIZE];
            int n;
            while ((n = in.read(buf)) >= 0)
                out.write(buf, 0, n);
        } finally {
            out.close();
        }
    } finally {
        in.close();
    }
}
```

심지어 위 얘시처럼 try-finally 문을 다중으로 중첩해서 사용한다면 코드가 지져분해지고 디버깅도 매우 힘들어진다.

따라서 이런 문제들을 해결하기 위해 자바 7에서 **try-with-resources** 문이 추가되었다.

```java
static String firstLineOfFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(
            new FileReader(path))) {
        return br.readLine();
    }
}
```

위에서 try-finally 문을 사용했던 예시들을 try-with-resources 문을 사용하여 표현하였다. 

이로서 전보다 훨씬 가독성이 좋아졌고 디버깅이 쉬워졌다.

```java
static final int BUFFER_SIZE = 4096;
static void copy(String src, String dst) throws IOException {
    try (InputStream in = new FileInputStream(src);
         OutputStream out = new FileOutputStream(dst)) {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
        while ((n = in.read(buf)) >= 0)
            out.write(buf, 0, n);
    }
}
```

보통의 try-finally 문처럼 try-with-resources 문에서도 catch 절을 사용할 수 있다.


```java
static String firstLineOfFile(String path, String defaultVal) {
    try (BufferedReader br = new BufferedReader(
            new FileReader(path))) {
        return br.readLine();
    } catch (IOException e) {
        return defaultVal;
    }
}
```

## 핵심 정리

꼭 회수해야 하는 자원을 다룰 때는 try-finally 말고 try-with-resources를 사용하자.

- 참고 : 이펙티브 자바 3/E

