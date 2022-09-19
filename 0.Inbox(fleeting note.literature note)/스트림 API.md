# 날짜 : 2022-08-17 07:37

# 주제 : #java #java8  #스트림API #모던자바인액션
----
# 메모
```toc
```

```ad-note
title: tl;dr
스트림이란 한 번에 한 개씩 만들어지는 연속적인 데이터 항목들의 모임
```


> 스트림이란 한 번에 한 개씩 만들어지는 연속적인 데이터 항목들의 모임이다 (p.44)
> 우선은 스트림 API가 조립 라인처럼 어떤 항목을 연속으로 제공하는 어떤 기능이라고 단순하게 생각하자 (p.45)


예를 들어, 자바의 System.in(표준입력) 에서 데이터를 읽은 다음, 데이터를 처리하고 결과를 System.out(표준 출력)으로 기록한다. 


## Stream
- 데이터를 담고 있는 저장소(컬렉션)이 아니다.
- Functional in nature(근본적으로 함수적이다), 스트림이 처리하는 `데이터 소스를 변경하지 않는다.`
- 스트림으로 처리하는 데이터는 오직 `한번만 처리`한다.
- 무제한일 수도 있다. (Short Circuit 메소드를 사용해서 제한할 수 있다.)
- [중개 오퍼레이션 (intermediate operation)](중개%20오퍼레이션%20(intermediate%20operation).md)은 근본적으로 lazy하다.
	- [종료 오퍼레이션 (terminal operation)](종료%20오퍼레이션%20(terminal%20operation).md)이 오기 전까지는 실행되지 않는다.
- 손쉽게 **병렬 처리**할 수 있다.
	- 스트림을 쓰지 않을 때와 가장 큰 차이점이다. 
	- for 문은 루프를 돌면서 병렬적으로 처리하기 어렵다. 하지만 스트림의 경우 [parallelStream](parallelStream.md)()을 받아서 처리하면 spliterator를 사용하여 병렬적으로 처리를 해준다.


## 스트림 파이프라인
- 연속된 데이터를 처리하는 오퍼레이션들의 모음. (그 자체가 데이터는 아님)
- 0 또는 다수의 [중개 오퍼레이션 (intermediate operation)](중개%20오퍼레이션%20(intermediate%20operation).md) 과 한 개의 [종료 오퍼레이션 (terminal operation)](종료%20오퍼레이션%20(terminal%20operation).md)으로 구성한다.
- 스트림의 데이터 소스는 오직 [종료 오퍼레이션 (terminal operation)](종료%20오퍼레이션%20(terminal%20operation).md)을 실행할 때에**만** 처리한다.
	- 종료 오퍼레이션이 없이 중개 오퍼레이션만 있으면 정의만 된 것일뿐, 실행되지 않기 때문에 무의미하다. 



## 스트림의 장점
자바 8에는 java.util.stream 패키지에 스트림 API가 추가되었다. 이 스트림 API의 핵심은 기존에는 한 번에 한 항목을 처리했지만 이제 자바 8에서는 하려는 **작업을 고수준으로 추상화해서 일련의 스트림으로 만들어 처리**할 수 있다는  점이다. 
또한, 스트림 파이프라인을 이용해서 입력 부분을 여러 CPU 코어에 쉽게 할당할 수 있다는 부가적인 이등도 있다. 
스레드라는  복잡한 작업을 사용하지 않고도 [[공짜로 병렬성을 얻을 수 있다]]

```ad-note
title: 컬렉션과 스트림 비교
1.
`컬렉션`에서는 for-loop 반복 과정을 직접 처리해야 했다 (외부 반복). 
하지만 **스트림 API**를 사용하면 스트림 API 라이브러리 내부에서 모든 데이터가 처리되므로 루프를 신경 쓸 필요가 없다. (내부 반복)

2.
`컬렉션`의 경우 거대한 리스트를 처리해야 할 때 단일 CPU로는 이 거대한 데이터를 처리하기 힘들다.
하지만 **스트림**으로는 멀티코어 컴퓨터 환경에서 서로 다른 CPU 코어에 작업을 각각 할당해서 처리 시간을 줄일 수 있다. 

```

즉, 자바 8은 **스트림 API**로 `'컬렉션을 처리하면서 발생하는 모호함과 반복적인 코드 문제' `그리고 `'멀티코어 활용 어려움'`이라는 두 가지 문제를 모두 해결했다.


## Stream API
### 걸러내기
1. Filter( Predicate )
	- 예) spring 으로 시작하는 수업
	```java
	springClasses.stream()  
        .filter( oc -> oc.getTitle().startsWith( "spring" ) )  
        .forEach( oc -> System.out.println( oc.getTitle() ) );
	```

### 변경하기
1. Map( Function ) 또는 FlatMap( Function )
	- 예) 두 수업 목록에 들어있는 모든 수업 아이디 출력 (  List<List<...>>을 List로 변경 )
	```java
	keesunEvents.stream()  
        .flatMap( Collection::stream ) // .flatMap( list -> list.stream() )  
        .forEach( oc -> System.out.println( oc.getId() ) );
	```

### 생성하기
1. generate( Supplier ) 또는 Iterate( T seed, UnaryOperator )
	- 예) 랜덤 int 무제한 스트림, 10부터 1씩 증가하는 무제한 스트림 등등
	```java
	// 10부터 1씩 증가하는 무제한 스트림 중에서 앞에 10개 빼고 최대 10개 까지만
	Stream.iterate( 10, i -> i + 1 )
        .skip( 10 )  
        .limit( 10 )  
        .forEach( System.out::println );  
;
	```

### 제한하기
1. limit( long ) 또는 skip( long )
	- 예) 최대 5개의 요소가 담신 스트림 리턴
	- 예) 앞에서 3개를 뺀 나머지 스트림 리턴

---

### 특정 조건을 만족하는 데이터 확인
1. anyMatch(), allMatch(), nonMatch()
	- 예) k로 시작하는 문자열이 있는지 확인한다 (true / false 리턴 )
	- 예) 스트림에 있는 모든 값이 10보다 작은지 확인
	```java
	System.out.println(":::::::::::: 자바 수업 중에 Test가 들어있는 수업이 있는지 확인");  
	boolean test = javaClasses.stream().anyMatch( jc -> jc.getTitle().contains( "Test" ) );
	```

### 갯수 세기
1. count()
- 예) 10보다 큰 수의 개수를 센다.

### 스트림을 데이터 하나로 뭉치기
1. reduce( identity, BiFunction ), collect(), sum(), max()
	- 예) 모든 숫자 합 구하기
	- 예) 모든 데이터를 하나의 List 또는 Set에 옮겨 담기





# 출처(참고문헌)
- [모던자바 인 액션] chapter1 - 자바 8, 9, 10, 11 : 무슨 일이 일어나고 있는가?

# 연결문서
- 모던 자바인 액션 4 ~ 7장
