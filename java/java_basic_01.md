
# Java란

자바는 **운영체제 즉 플랫폼에 독립적으로 어디에서나 실행이 가능한 프로그램을 제공하는 언어**이다.
처음 **오크(Oak)** 라는 이름으로 시작되어 썬 마이크로시스템즈의 개발자들로부터 개발되었다.
자바의 원래 목표는 가전제품에 탑재될 소프트웨어를 만드는 것이었다. 따라서 초기에는 가전제품이나 PDA와 같은 소형기기에 사용될 목적으로 같은 코드를 여러 종류의 운영체제에서 동일하게 구동이 가능한 것이 큰 특징이었다. 이후 다양한 운영체제를 사용하는 컴퓨터들이 인터넷 통신을 하게되면서 자바도 웹 개발에 적합하게 개발이 되었다.

### 자바 언어의 특징
- 운영체제 독립적이다.
- 객체지향언어이다.
- 자동 메모리 관리(Garbage Collection)가 가능하다.
- 멀티쓰레드를 지원한다.
- WORA(Write Once, Run Anywhere)
- 동적 로딩(Dynamic Loading)을 지원한다.
> 동적 로딩 : 자바는 여러개의 클래스로 구성되어 있는데, 실행시에 모든 클래스가 한번에 로딩되지 않고 필요한 시점에 클래스를 로딩하여 사용할 수 있다.

### JDK(Java Development Kit)

자바 프로그램 개발시 필요한 도구의 모음이다. JDK 내부에는 JVM과 자바 클래스 라이브러리외에 자바 개발에 필요한 프로그램들이 있다.
JDK는 용도에 따라 종류가 다양하고 각 용도에 필요한 API가 추가되는 차이가 있다.
> JDK = JRE + 자바 개발 도구

- JDK 종류
  - Java SE(Standard Edition) : 표준 자바 플랫폼
  - Java EE(Enterprise Edition) : 상업 용도의 자바 플랫폼
  - Java ME(Micro Edition) : 임베디드 기기 용도의 자바 플랫폼

> - 자바 주요 실행 파일들
>   - javac : 자바 컴파일러, 자바 소스코드를 바이트코드로 컴파일한다.
>   - java : 자바 인터프리터, 컴파일러가 생성한 바이트코드를 해석하고 실행한다.
>   - javap : 역어셈블러, 컴파일된 클래스파일을 원래의 소스로 변환한다.
>   - javadoc : 자동문서 생성기, 소스파일에 있는 주석을 이용하여 자바 API문서와 같은 형식의 문서를 자동으로 생성한다.
>   - jar : 압축 프로그램, 클래스 파일과 프로그램의 실행에 관련된 파일을 하나의 jar 파일(.jar)로 압축하거나 압축해제한다.

![](https://images.velog.io/images/host92/post/089f5085-4ef8-46b3-acf3-dedf5757a84d/image.png)
- 출처 : https://www.oracle.com/java/technologies/platform-glance.html

### JRE(Java Runtime Enviroment)

자바 실행 환경으로 자바로 작성된 응용 프로그램이 실행되기 위한 최소 환경으로 자바 프로그램을 실행 시키려면 JDK 없이 JRE만 있어도 실행이 가능하다.
> JRE = JVM + Java API

### JVM(Java Virtual Machine)

자바를 실행하기 위한 가상 머신으로 자바는 JVM만 제공된다면 그 어떤 운영체제에서도 실행이 가능하다.
JVM은 Windows, Linux, Macos 각각의 운영체제에 맞는 JVM이 존재한다.

> 가상 머신이란 프로그램을 실행하기 위해 물리적 머신(즉, 컴퓨터)와 유사한 머신을 소프트웨어로 구현한 것을 말한다.


- JVM의 역할
  - 인터프리터
  - GC(Garbage Collection)를 활용한 힙 메모리 관리

- JVM의 요소
  - 인터프리터
  - 클래스로더
  - 실행 엔진(JIT 컴파일러)
  - GC(Garbage Collection)

![](https://images.velog.io/images/host92/post/4083b53c-4d61-4b9b-9c26-ef5106e8a8f0/image.png)


#### 자바 프로그램 컴파일 및 실행 과정

![](https://images.velog.io/images/host92/post/09b55346-7a6b-44d9-ab33-888f038d598f/image.png)

---------------

이 글은 자바 언어에 대한 기본기를 다지기 위해 작성하는 글입니다. 
글에서 잘못되거나 추가되어야 하는 내용 관련 사항은 jungwoo5759@gmail.com 로 공유해주시면 감사하겠습니다.

- 참고 
  - https://d2.naver.com/helloworld/1230
  - https://github.com/whiteship/live-study/issues/1
  - https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%9C%EB%B0%9C_%ED%82%A4%ED%8A%B8
  - https://www.oracle.com/java/technologies/platform-glance.html
  - 자바의 정석 책 3rd Edition
  
