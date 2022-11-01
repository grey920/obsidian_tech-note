# 날짜 : 2022-10-11 07:53

# 주제 : #java 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```

## 애노테이션이란
애노테이션은 클래스나 메소드 등의 선언시에 @를 사용하는 것으로 메타데이터라고도 불린다.
( 애노테이션은 클래스, 메소드, 변수, 예외타입 등 모든 요소에 선언할 수 있다 )

## 애노테이션, 언제 사용할까?
- 컴파일러에게 정보를 알려주거나
- 컴파일할 때와 설치(deployment)시의 작업을 지정하거나
- 실행할 때 별도의 처리가 필요할 때


## 이미 정해져 있는 3가지 애노테이션
### @Override
- 해당 메소드가 부모 클래스에 있는 메소드를 오버라이드했다는 것을 명시적으로 선언한다.
- 왜 사용할까
	- 1. 만약 자식 클래스에 여러 메소드가 있다면, 어떤 메소드가 오버라이드 되었는지 쉽게 알 수 없다.
	- 2. 제대로 오버라이드 했다고 생각했지만 매개 변수가 하나 빠지거나 하는 등의 실수가 있을 수 있다. 
	- 따라서  명확하게 부모 클래스로부터 이 메소드가 오버라이드된 것임을 명시해서 컴파일러 단계에 체크할 수 있도록 지정하는 것이다. ( 자바의 신1 - 17장 433p)


### @Deprecated
- 더 이상 사용되지 않는 클래스나 메소드의 경우 명시적으로 선언하여 어디선가 사용하는 경우 컴파일러가 경고할 수 있도록 한다.  
- 왜 사용할까
	- 사용하지 않는다고 바로 지워버린다면,  그 클래스 혹은 메소드를 참조하는 다른 개발자가 만든 프로그램이 변경된 사항을 모르고 있을 때 컴파일 하는 단계에서 에러가 발생할 것이다. 따라서, `하위 호환성을 위해서` Deprecated로 선언하는 것은 꼭 필요하다.
	- 가장 좋은 방법은 계도 기간을 거쳐 알림을 준 후에 지우는 것이 바람직하다


### @SuppressWarnings
- 프로그램에는 문제가 없지만 코딩을 하다가 컴파일러에서 경고(warning)을 알리는 경우, 컴파일러에게 내가 의도적으로 이렇게 코딩한 것임으로 warning을 줄 필요가 없음을 알려주는 것이다. 
- 다른 애노테이션들과 다르게 문자열을 넘겨 주어 속성값을 지정할 수 있다.
	```java
	@SuppressWarnings("deprecation")
	```



## 애노테이션을 선언하기 위한 메타 애노테이션
[메타 애노테이션 (Meta annotation)](메타%20애노테이션%20(Meta%20annotation).md)이라는 것은 애노테이션을 선언할 때 사용한다. 



## 애노테이션과 리플렉션
### 중요 애노테이션
- `@Retention`: 해당 애노테이션을 언제까지 유지할것인가
	- 애노테이션은 기본적으로 '주석'과 같다. 따라서 바이트코드를 로딩했을 때 애노테이션 정보는 빼고 읽어오므로, **애노테이션 정보까지 메모리에 올리고 싶다면 @Retention 전략**을 써야 한다. 
	- 소스( RetentionPolicy.SOURCE ), 클래스( RetentionPolicy.CLASS ), 런타임( RetentionPolicy.RUNTIME )
	- 기본값은 CLASS
- `@Inherit`: 해당 애노테이션을 하위 클래스까지 전달할 것인가?
	- 애노테이션을 사용하는 객체의 자식 객체들도 적용되게 하려면 @Inherited 를 사용한다
- `@Target`: 어디에 사용할 수 있는가?
	- 해당 애노테이션을 붙일 위치를 지정한다. 
	- @Target( {ElementType.TYPE , ElementType.FIELD , ElementType.CONSTRUCTOR })

### 애노테이션에 값 주기
- 애노테이션은 primitive로 제한된 값들을 가질 수 있다. 
- 기본값을 주려면 `default` 키워드를 사용한다.
- value() 는 특별히 애노테이션을 사용할 때 다른 값들처럼 따로 이름을 명시하지 않아도 된다. 
	- 예. `@MyAnnotation("grey")`
	- 하지만 여러개의 파라미터를 보낸다면 value도 명시해야 한다. 
	- 값을 하나만 필요로할 때 유용
```java
@Retention( RetentionPolicy.SOURCE )
@Target( {ElementType.TYPE , ElementType.FIELD , ElementType.CONSTRUCTOR })
@Inherited 
public @interface MyAnnotation {  
	String value() default "grey";  
  
	String name() default "grey";  
  
	int number() default 100;  
}
```
	

### [리플렉션](리플렉션.md)
- `getAnnotations()`: 상속받은 (@Inherit) 애노테이션까지 조회
	```java
	Arrays.stream( MyBook.class.getAnnotations() ).forEach( System.out::println );
	```

- `getDeclaredAnnotations()`: 자기 자신에만 붙어있는 애노테이션 조회션
	```java
	Arrays.stream( MyBook.class.getDeclaredAnnotations() ).forEach( System.out::println );
	```

- 필드에 붙은 애노테이션 찾기 & 애노테이션 값 조회
	```java
	Arrays.stream( Book.class.getDeclaredFields() ).forEach( f -> {  
	    Arrays.stream( f.getAnnotations() ).forEach( a -> {  
	        if ( a instanceof MyAnnotation ) {  
	            MyAnnotation myAnnotation = ( MyAnnotation ) a;  
	            System.out.println( myAnnotation.value() );  
	            System.out.println( myAnnotation.name() );  
	            System.out.println( myAnnotation.number() );  
	        }  
	    } );  
	} );
	```



## [애노테이션 프로세서](애노테이션%20프로세서.md)





# 출처(참고문헌)
- 

# 연결문서
- 
