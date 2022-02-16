
추상 클래스와 인터페이스는 자바에서 클래스를 정의할 때 사용되는 특별한 클래스이다. 일반적인 클래스처럼 멤버 변수와 메서드를 가질 수 있지만 객체를 생성하지 못하고 특정 클래스에 **상속(extends)** 되거나 **구현(implements)** 되어 사용된다.

추상 클래스와 인터페이스는 객체로 생성하지 않고 개발 과정에서 프로그래머에게 클래스 가독성과 구현을 쉽게하도록 도움을 준다.

# 추상 클래스(abstract class)

일반 클래스가 객체를 생성하기 위한 '설계도'라고 한다면 추상 클래스는 설계도를 정의하는데 도와주는 '미완성의 설계도'라고 한다. 따라서 추상 클래스는 객체로서 사용되지 않고 클래스가 정의될 때 미리 필요한 **공통의 멤버나 메서드를 만들어서 클래스(설계도)를 만드는데 방향을 제시해 줄 수 있다.**

> **추상[抽象]** : 여러 가지 사물이나 개념에서 **공통되는 특성이나 속성 따위를 추출**하여 파악하는 작용.

### 추상 클래스 정의와 구성

추상 클래스는 class 선언 전에 **abstract 키워드**를 선언하면 된다.

>
추상 메서드를 포함하지 않은 온전한 클래스에도 `abstract` 키워드를 선언하면 추상 클래스로 지정할 수 있다.


- 추상 클래스 정의

```java
abstract class 클래스이름 {  // 추상 클래스 선언
    ...
}
```

- 추상 메서드 정의

```java
/* 주석을 통해 어떤 기능을 수행할 목적으로 작성하였는지 설명한다. */
abstract 리턴타입 메서드이름();
```

추상 클래스의 구성은 일반 클래스와 동일하게 멤버 변수, 메서드, 생성자를 가질 수 있다.
일반 클래스와 차이점은 추상 메서드를 포함한다는 것이다.

- 추상 클래스 사용 예시
```java
public class PlayerTest {
    public static void main(String[] args) {
        CDPlayer cdPlayer = new CDPlayer();
        System.out.println(cdPlayer.toString());
        cdPlayer.play(100);
        System.out.println(cdPlayer.toString());
        cdPlayer.pause();
        System.out.println(cdPlayer.toString());
        cdPlayer.currentPos = 50;
        cdPlayer.pause();
    }
}

// 추상 클래스
abstract class Player { // 추상 클래스
    boolean pause;      // 일시정지 상태를 저장하기 위한 변수
    int currentPos;     // 현재 Play되고 있는 위치를 저장하기 위한 변수

    Player() {          // 추상 클래스도 생성자가 있어야 한다.
        pause = false;
        currentPos = 0;
    }
    /** 지정된 위치(pos)에서 재생을 시작하는 기능이 수행되도록 작성되어야 한다. **/
    abstract void play(int pos);  // 추상 메서드 - 구현부(몸통)x
    /** 재생을 즉시 멈추는 기능을 수행하도록 작성되어야 한다. **/
    abstract void stop();         // 추상 메서드

    void play() {
        play(currentPos);         // 추상메서드를 사용할 수 있다.
    }
    void pause() {
        if (pause) {  // pause가 true일 때(정지상태)에서 pause가 호출되면
            pause = false;     // pause의 상태를 false로 바꾸고
            play(currentPos);  // 현재 위치에서 play를 한다.
        } else {  // pause가 true일 때(play 상태)에서 pause가 호출되면
            pause = true;  // pause의 상태를 true로 바꾸고
            stop();        // play를 멈춘다.
        }
    }
}

// Player 추상 클래스를 상속하는 CDPlayer 자식 클래스
class CDPlayer extends Player {  // 추상 클래스 상속
    void play(int pos) {
        /* 부모의 추상 메서드 내용 구현 */
        System.out.println("Play player currentPos:" + pos);
        super.currentPos = pos;
    }
    void stop() {
        /* 부모의 추상 메서드 내용 구현 */
        System.out.println("Stop player");
        super.currentPos = 0;
    }

    // CDPlayer 클래스에 추가로 정의된 멤버와 메서드
    int currentTrack;  // 현재 재생중인 트랙
    void nextTrack() {
        currentTrack++;
    }
    void preTrack() {
        if (currentTrack > 1) {
            currentTrack--;
        }
    }

    @Override
    public String toString() {
        return "CDPlayer{" +
                "pause=" + pause +
                ", currentPos=" + currentPos +
                '}';
    }
}
```

- 추상 클래스 예제 실행 결과
```
CDPlayer{pause=false, currentPos=0}
Play player currentPos:100
CDPlayer{pause=false, currentPos=100}
Stop player
CDPlayer{pause=true, currentPos=0}
Play player currentPos:50
```


추상 메서드는 메서드 앞에 **abstract 키워드**를 선언만하고 구현은 하지 않는다. 구현부는 상속 받는 클래스에서 **꼭 재정의해야 한다.**

>**추상화** : 클래스 간의 공통점을 찾아내서 공통의 부모를 만드는 작업<br>
>**구체화** : 상속을 통해 클래스를 구현, 확장하는 작업

### 추상 클래스의 상속

추상 클래스는 같은 추상 클래스가 상속 받을 수 있다. 추상 클래스를 상속 받은 자식 클래스가 추상 메서드를 호출하면 추상 클래스가 된다.<br>
**추상 클래스를 상속 받은 추상 클래스는 추상 메서드를 재선언하지 않아도 된다. 추상 클래스를 상속 받는 클래스만 추상 메서드를 구현하기만 하면된다.** 

```java
abstract class Player {
    abstract void play(int pos);
    abstract void stop(); 
    ...
}
// 추상 클래스를 상속 받은 추상 클래스
abstract class CDPlayer extends Player {  
    abstract void play(int pos);  // 추상 메서드 재선언 -> 추상 클래스
    // stop() 재선언x
    ...
}

class CDPlayerMachine extends CDPlayer {
    void play(int pos) { ... };
    void stop() { ... }; 
    ...
}
```

> 추상 메서드를 선언하면 무조건 추상 클래스가 된다.

# 정리

![](https://images.velog.io/images/host92/post/5e0a3057-baff-47fb-b2f1-20a7363a66a5/image.png)

-------------------------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다.
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.
해당 글을 참고하시거나 퍼가실 때는 출처 링크 부탁드립니다 :)

참고
1. [백기선-자바스터디](https://github.com/whiteship/live-study)
2. 이것이 자바다 - 신용권의 Java 프로그래밍 정복
3. 자바의 정석 책 3rd Edition
