## 날짜 : 2022-09-11 18:08

## 주제 : #java8 #함수형프로그래밍 
----
## 메모
```ad-note
title: tl;dr
- 자바에서 미리 정의해둔 자주 사용할만한 함수 인터페이스
```

### 자바에서 왜 기본 함수형 인터페이스를 제공하는 걸까?
- 함수형 인터페이스는 단 하나의 추상메서드만 가지고 있어야 한다. 그렇기 때문에 형태가 꽤나 제한적이다.
- 매개변수 하나에 리턴이 있는 메서드( Function ), 리턴은 없고 매개변수만 있는 메서드(Consumer), 매개변수가 없고 리턴만 있는 메서드( Supplier ) 등 ***어느정도 예측 가능한 형태의 메서드***는 개발자의 편의를 위해 미리 자바에서 제공하는 것.


### [java.util.function 패키지](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html)
- Function<T,R>: T 타입을 받아서 R 타입을 리턴하는 함수 인터페이스
	- apply(T t): 받은 매개변수를 적용한다
	- 함수 조합용 메서드
		- Function<T,V> **andThen**( Function after ): andThen 앞의 함수 적용 후 after 함수를 적용한 조합 함수를 리턴
		- compose( Function before )
			```java
			/* Function<T,R>*/  
			Function<Integer, Integer> plus5 = ( i ) -> i + 5;  
			Function<Integer, Integer> multiply5 = ( i ) -> i * 5;  
			  
			Function<Integer, Integer> plus5AndMultiply5 = plus5.andThen( multiply5 );  
			System.out.println( plus5AndMultiply5.apply( 2 ) );  
			  
			Function<Integer, Integer> plus5AndcomposeMultiply5 = plus5.compose( multiply5 );  
			System.out.println( plus5AndcomposeMultiply5.apply( 2 ) );

			
			>> 35
			>> 15
			```
		
- Consumer< T >: T 타입을 받고 리턴이 없는 함수 인터페이스 (void)
	- accept( T t )
	- andThen( Consumer after )
- Supplier< T >: 매개변수가 없고 T 타입으로 리턴하는 함수 인터페이스 
	- get()
- Predicate< T >: T 타입을 받고 boolean값을 리턴하는 함수 인터페이스
	- and( Predicate other)
	- or( Predicate other )
	- test( T t )
	- isEqual( Object targetRef )
	```java
	Predicate<Integer> isOdd = ( i ) -> i % 2 != 0;  
	System.out.println( isOdd.test( 4 ) );  
	// negate(): 반대의 값을 리턴하는 메서드  
	Predicate<Integer> negate = isOdd.negate();  
	System.out.println( negate.test( 4 ) );
	```
- UnaryOperator< T >
	- Function< T, R >의 특수한 형태로, 입력값과 리턴값이 동일한 타입인 경우 하나로 쓸 수 있는 함수 인터페이스



## 출처(참고문헌)
- 

## 연결문서
- 
