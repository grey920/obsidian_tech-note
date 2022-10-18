# 날짜 : 2022-10-18 07:40

# 주제 : #java #code_manipulation
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```


## 1. 용어 정리 
![](../img/IMG_9BB70C6FE328-1.jpeg)
### JVM (Java Virtual Machine)
자바 가상 머신으로, 자바 `바이트 코드(.class)`를 어떻게 실행할지에 대한 표준 스팩이다.
-  **자바 바이트 코드( .class 파일 )를 -> OS에 특화된 코드로 변환하여 실행**한다.
	- 인터프리터와 JIT 컴파일러가 Native OS(mac, windows,...)에 맞게 머신코드로 변환하여 실행함
	- 예를 들어, Hello.class 클래스 파일만 있으면 명령어로 `java Hello`을 쳐서 해당 클래스 파일을 실행할 수 있다 
	- ![](../img/Pasted%20image%2020221018195341.png)
- JVM이 하는 일
	- 클래스 읽기, 메모리에 올리기, OS에 맞는 머신 코드로 변환해서 실행하기, ...
- JVM 스팩 : https://docs.oracle.com/javase/specs/jvms/se11/html/
- JVM 밴더 : 오라클, 아마존, Azul, ...
	- JVM 스팩에 맞춰 다양하게 구현할 수 있다.
- 특정 **플랫폼에 종속적**이다.
	- 네이티브 코드로 바꿔서 실행해야 하는데 이게 OS에 맞게 변환해야 하므로...

### JRE (Java Runtime Environment) : JVM + 라이브러리
- `자바 애플리케이션을 실행`할 수 있도록 구성된 배포판. ( 최소한의 배포단위로 실행에 필요한 것만 들어있다 )
	- JVM이 단독으로 제공되지 않는다.
- JVM과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 셋팅이나 리소스 파일을 가지고 있다.
- 개발에 필요한 툴은 포함하지 않는다 -> 그건 JDK에서 제공한다
	- javac 컴파일러는 없다. 그래서 JRE만으로 class파일을 실행할 수는 있지만 class파일로 컴파일 할 수는 없다. 


### JDK (Java Development Kit) : JRE + 개발 툴
- JVM < JRE < JDK
- 소스 코드를 작성할 때 사용하는 자바 언어는 플랫폼에 독립적
	- Write Once Run Anywhere
- 오라클은 자바 11부터는 JDK만 제공하며 JRE는 따로 제공하지 않는다
	- 예전에는 버전별로 JRE와 JDK 배포판을 두 개씩 제공했다.
	- 자바 9부터 java module system이 들어왔다. 이를 가지고 나만의 작은 JRE같은 jlink를 만들 수 있다


### 자바
- 프로그래밍 언어
- JDK에 들어있는 자바 컴파일러 (javac)를 사용하여 바이트코드(.class)로 컴파일 할 수 있다. 
	- 자바 컴파일러(`javac Hello.java`) : 자바 코드 -> 바이트 코드 (.class)
	- 인터프리터와 JIT(`java Hello`) : 바이트 코드 (.class) -> OS 특화 코드
- 자바 유료화
	- 오라클에서 만든 `Oracle JDK 11 버전`부터 상용으로 사용할 때 유료
	- 오라클에서 만든 Open JDK나 다른 밴더에서 만든 JDK는 무료


### JVM 언어
- JVM 기반으로 동작하는 프로그래밍 언어
	- JVM은 사실상 class파일만 있으면 실행을 할 수 있기 때문에 JAVA와의 의존성이 강하지 않다. 
	- 따라서 다른 언어일지라도 어떤 파일을 `컴파일했을 때 class파일이나 java 파일만 만들 수 있다면` JVM을 활용할 수 있다.
- 클로저, 그루비, JRuby, Jython, Kotlin, Scala, ...
	- 코틀린 파일을 jar로 패키징해서 실행할 수 있다 : `kotlinc Hello.kt -include-runtime -d hello.jar`


## 2. JVM 구조
### 클래스 로더 시스템

### 메모리

### 실행 엔진

### JNI (Java Native Interface)

### 네이티브 메소드 라이브러리


## 3. 클래스 로더 



# 출처(참고문헌)
- "코드를 조작하는 다양한 방법"
	-   JIT 컴파일러: [https://aboullaite.me/understanding-jit-compiler-just-in-time-compiler/](https://aboullaite.me/understanding-jit-compiler-just-in-time-compiler/)
	-   JDK, JRE 그리고 JVM: [https://howtodoinjava.com/java/basics/jdk-jre-jvm/](https://howtodoinjava.com/java/basics/jdk-jre-jvm/)
	-   [https://en.wikipedia.org/wiki/List_of_JVM_languages](https://en.wikipedia.org/wiki/List_of_JVM_languages)


# 연결문서
- 
