# 날짜 : 2022-10-12 07:34

# 주제 : #java 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 애노테이션을 직접 선언할 때 사용하는 애노테이션
```


## 메타 애노테이션의 종류

^13e5af

애노테이션을 선언할 떄 꼭 이 4개를 모두 사용해야 하는 것은 아니지만 이 4개가 있다는 것은 알아두면 좋다. 

### @Target
- 애노테이션을 어떤 것에 적용할지를 선언할 때 사용한다. 
- 적용 방법 : @Target() 괄호 안에 적용 대상을 지정한다.
	```java
	@Target({ElementType.TYPE, ElementType.METHOD})  
	@Retention(RetentionPolicy.RUNTIME)  
	@Inherited  
	@Documented  
	public @interface Transactional {  
	    @AliasFor("transactionManager")  
	    String value() default "";
	}
	```
- [적용 가능한 대상 목록](https://docs.oracle.com/javase/8/docs/api/java/lang/annotation/ElementType.html)

	| 요소 타입      | 대상                               |
	| -------------- | ---------------------------------- |
	| CONSTRUCTOR    | 생성자 선언시                      |
	| FIELD          | enum 상수를 포함한 필드값 선언시   |
	| LOCAL_VARIABLE | 지역 변수 선언시                   |
	| METHOD         | 메소드 선언시                      |
	| PACKAGE        | 패키지 선언시                      |
	| PARAMETER      | 매개 변수 선언시                   |
	| TYPE           | 클래스, 인터페이스, enum 등 선언시 |
	| TYPE_PARAMETER | `타입 변수(타입 파라미터)`에만 사용                                   |
	| TYPE_USE       | 타입 파라미터를 포함해서 `모든 타입 선언부`                                 |

	- TYPE_PARAMETER와 TYPE_USE는 자바 8에서 생긴 변화

### @Retention
- 얼마나 오래 애노테이션 정보가 유지되는지를 선언한다.
- 적용 방법: @Retention() 괄호 안에 적용 대상을 지정한다.
	```java
	@Retention(RetentionPolicy.RUNTIME)
	```
- 적용 가능한 대상 목록
|         | 대상                                                                                              |
| ------- | ------------------------------------------------------------------------------------------------- |
| SOURCE  | 애노테이션 정보가 컴파일시 사라짐                                                                 |
| CLASS   | 클래스 파일에 있는 애노테이션 정보가 컴파일러에 의해서 참조 가능함. 하지만 가상 머신에서는 사라짐 |
| RUNTIME | 실행시 애노테이션 정보가 가상 머신에 의해서 참조 가능함                                                                                                  |

### @Documented
- 해당 애노테이션에 대한 정보가 Javadocs(API) 문서에 포함된다는 것을 선언한다


### @Inherited
- 모든 자식 클래스에서 부모 클래스의 애노테이션을 사용 가능하다는 것을 선언한다.

```ad-important
title: @interface
@interface 는 애노테이션을 선언할 때 사용한다.
클래스나 인터페이스를 선언할 때처럼 @interface로 선언하면 해당 클래스로 애노테이션 사용이 가능해진다
- 예) public @interface UserAnnotation{ ... }
```



# 출처(참고문헌)
- 

# 연결문서
- [자바 8의 애노테이션의 변화](자바%208의%20애노테이션의%20변화.md)
- 
