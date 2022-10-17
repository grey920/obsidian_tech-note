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
자바 바이트 코드를 어떻게 실행할지에 대한 표준 스팩
- 자바 가상 머신으로 **자바 바이트 코드( .class 파일 )를 -> OS에 특화된 코드로 변환하여 실행**한다.
	- OS에 특화된 코드로 변환은 인터프리터와 JIT 컴파일러가 한다
- JVM 스팩 : https://docs.oracle.com/javase/specs/jvms/se11/html/
- JVM 밴더 : 오라클, 아마존, Azul, ...
- 특정 **플랫폼에 종속적**이다.

### JRE (Java Runtime Environment) : JVM + 라이브러리
- 자바 애플리케이션을 실행할 수 있도록 구성된 배포판
- JVM과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 셋팅이나 리소스 파일을 가지고 있다.
- 개발 관련 도구는 포함하지 않는다 -> 그건 JDK에서 제공한다


### JDK (Java Development Kit) : JRE + 개발 툴
- JVM < JRE < JDK
- 소스 코드를 작성할 때 사용하는 자바 언어는 플랫폼에 독립적
	- Write Once Run Anywhere
- 오라클은 자바 11부터는 JDK만 제공하며 JRE는 따로 제공하지 않는다


### 자바
- 프로그래밍 언어
- JDK에 들어있는 자바 컴파일러 (javac)를 사용하여 바이트코드(.class)로 컴파일 할 수 있다. 
	- 자바 컴파일러 : 자바 코드 -> 바이트 코드 (.class)
	- 인터프리터와 JIT : 바이트 코드 (.class) -> OS 특화 코드
- 자바 유료화
	- 오라클에서 만든 Oracle JDK 11 버전부터 상용으로 사용할 때 유료


### JVM 언어
- JVM 기반으로 동작하는 프로그래밍 언어
- 클로저, 그루비, JRuby, Jython, Kotlin, Scala, ...


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
