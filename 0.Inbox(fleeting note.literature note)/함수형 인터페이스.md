## 날짜 : 2022-09-09 21:44

## 주제 : #java8 
----
## 메모
```ad-note
title: tl;dr
- 추상 메서드가 하나만 있는 인터페이스를 **함수형 인터페이스**라 한다.
```


### 코드
```java
package me.whiteship.java8to11;  

//컴파일 단계에서 내부를 확인하여 인터페이스를 더 견고하게 관리할 수있으니 함수형 인터페이스라면 붙이는 게 좋다. 
@FunctionalInterface 
public interface RunSomthing {  
  
		// 추상메서드 ( abstract 키워드 생략함) -> 추상메서드는 오직 하나여야 함.   
		void doIt();  
  
        // 새로운 기능! 인터페이스에 인터페이스임에도 static 메서드나 default 키워드로 메서드를 선언할 수 있다.  
        static void printName() {  
                System.out.println("grey");  
        }  
  
        default void printAge() {  
                System.out.println("31");  
        }  
}
```


## 함수형 인터페이스란 ( Functional Interface )
- 추상 메서드를 딱 하나만 가지고 있는 인터페이스
	- SAM (Single Abstract Method)  인터페이스
- @FunctionalInterface 애노테이션을 가지고 있는 인터페이스


## 정의한 함수형 인터페이스 사용 방법

```ad-note
인터페이스는 직접 객체화할 수 없기 때문에 구현 클래스를 이용하는데, 일회성으로 사용하는 구현 클래스를 계속 선언하는 것은 *비효출적*이기 때문에 `익명 클래스`나 `람다`를 이용하여 구현 클래스를 선언한다
```


1. 자바 8 이전 : [[익명 내부 클래스]] 사용
	```java
	package me.whiteship.java8to11;  
	
	public class Foo {  
	  
			public static void main ( String[] args ) {  
	  
					// 자바 8 이전 -> 익명 내부 클래스  
					RunSomthing runSomthing = new RunSomthing() {  
							@Override  
							public void doIt () {  
							System.out.println("Hello");
	  
							}                
					};  
	  
			}  
	}
	```

2. 자바 8 이후 : [람다 (Lambda Expressions)](람다%20(Lambda%20Expressions).md) 표현식 
	```java
	package me.whiteship.java8to11;  
  
	public class Foo {  
	  
	    public static void main ( String[] args ) {  
	  
	        // 자바 8 이후 -> 람다 표현식  
	        // 람다가 마치 함수 정의처럼 보이지만, 실질적으로는 특수한 형태의 오브젝트이다 
	        // 이 특수한 형태의 오브젝트를 변수에 할당하고 매개변수로 넘기거나 리턴할 수 있다는 뜻이다
	        RunSomthing runSomthing = () -> System.out.println( "Hello" );  
	  
	    }  
	}
	```


## [[함수형 프로그래밍]]
![[함수형 프로그래밍#^0ef8b4]]

```java
public static void main ( String[] args ) {  
  
		// 자바 8 이후 -> 람다 표현식  
		RunSomthing runSomthing = () -> System.out.println( "Hello" );  
  
	}  
```




### 자바에서 기본으로 제공하는 함수형 인터페이스
[java.util.function 패키지](java.util.function%20패키지.md)



## 출처(참고문헌)
- 더 자바8 - 백기선
- [익명클래스 vs 람다](https://skasha.tistory.com/34)

## 연결문서
- 
