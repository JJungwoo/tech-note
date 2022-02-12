# Serializable을 구현할지는 신중히 결정하라

## **Serializable 구현할 때 위험성**

아래의 예시처럼 클래스에 대해 Serializable를 구현하는 것은 간단해 보일 수 있지만 보기보단 값비싼 비용이 들어간다.

```java
public class test implements Serializable {
	private Integer id;
	private String name;

  // 직렬화
  private void writeObject(ObjectOutputStream oos) {
      try {
          oos.defaultWriteObject();
          oos.writeObject(this.id);
          oos.writeObject(this.name);
      } catch (IOException e) {
          // ... 구현 생략
      }
  }

  // 역직렬화
  private void readObject(ObjectInputStream ois) {
      try {
          ois.defaultReadObject();
          this.id = (Integer)ois.readObject();
          this.name = (String)ois.readObject();
      } catch (IOException | ClassNotFoundException e) {
          // ... 구현 생략
      }
  }
}
```

- **1. Serializable을 구현하면 릴리스한 뒤에는 수정하기 어렵다**
  Serializable을 구현하면 내부 구현 방식(멤버 변수 정보 등)에 따라 코드가 변경되어야 한다(호환성 때문에 유지보수가 어렵다). <br>
  이때 private과 package-private 인스턴스 필드들 마저도 공개가 된다(캡슐화가 깨진다).
- **2. Serializable 구현의 두 번째 문제는 버그와 보안 구멍이 생길 위험이 높아진다는 점이다**
  item85 내용처럼 Serializable을 구현하는 객체 내부 코드에 대한 안전성이 보장되지 않는다.
- **3. Serializable 구현의 세 번째 문제는 해당 클래스의 신버전을 릴리스할 때 테스트할 것이 늘어난다는 점이다**
  직렬화 가능 클래스를 수정할 때마다 양방향 직렬화/역직렬화에 대한 동작 테스트가 늘어난다.

## **Serializable 구현 시 주의사항**

- **Serializable 구현 여부는 가볍게 결정할 사안이 아니다** <br>
  Serializable 구현에 대한 비용이 많이 들기 때문에 이득과 비용을 잘 저울질해야 한다.
- **상속용으로 설계된 클래스는 대부분 Serializable을 구현하면 안 되며, 인터페이스도 대부분 Serializable을 확장해서는 안 된다**
- **내부 클래스는 직렬화를 구현하지 말아야 한다**

# 핵심 정리

- Serializable는 매우 간단하지만 유지보수와 데이터 노출 등에 대한 위험 때문에 신중히 사용해야 한다.
