# 날짜 : 2022-09-21 07:07

# 주제 : #java8 #NPE 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 옵셔널(Optional)은 오직 값 한 개가 들어있을 수도, 없을 수도 있는 `컨테이너`
```


## 우리가 자바 프로그래밍에서 NPE를 만나는 이유는??
### -> null 을 리턴하니까 && null 체크를 깜빡했으니까
![](../img/Pasted%20image%2020220921073937.png)
- 기존의 null 체크 -> 이러한 null체크를 잊을 수 있기 때문에 에러를 만들기 좋은 코드라 좋지 않다. 
	```java
	OnlineClass spring_boot = new OnlineClass( 1, "spring boot", true );  
	Progress progress = spring_boot.getProgress();  
	if ( progress != null ){  
	    System.out.println( progress.getStudyDuration() );  
	}
	```
- 하지만 우리는 사람이니까 깜빡할 수 있다.. 

## Optional의 탄생 배경
### 1. 우리는 사람이니까 null 체크를 깜빡할 수 있다
### 2. null을 리턴하는것 자체가 문제다. 하지만 자바8 이전에는 별다른 해결책이 없었다. 
- 여기서 선택지는 1) 에러를 던지거나 2) null을 리턴하는 방법 이 두가지밖에 없었다.



## 언제 Optional을 리턴해야 할까?
### 메소드에서 작업 중 특별한 상황에서 값을 제대로 리턴할 수 없는 경우 선택할 수 있는 방법
1. `예외를 던진다` -> 예외가 발생한 부분까지 쭉 스택 트레이스를 찍기 때문에 비싸다
	1. checked Exception을 던지면 에러 처리를 강제해버린다. 괴로워..
	2. 예외가 발생한 부분까지 쭉 스택 트레이스를 찍기 때문에 이 자체로 리소스를 쓰게 되는 것이다. 따라서 진짜로 필요한 상황에서 써야지, 로직을 처리하기 위해 쓰는 것을 적절하지 않다. 
2. `null을 리턴`한다 -> null을 리턴하면 클라이언트쪽 코드에서 이를 체크해야 하기 때문에 이 자체로 위험하다
3. **(자바 8부터) Optional을 리턴한다.** 클라이언트 코드에게 명시적으로 빈 값일수도 있다는 걸 알려주고, 빈 값인 경우에 대한 처리를 강제한다.


## Optional로 객체 감싸기 ( of와 ofNullable )
### 1. Optional.ofNullable(  ) -> null일 수 있는 객체를 감싸는 경우 
```java
public Optional<Progress> getProgress () {  
    return Optional.ofNullable( progress ); // 문법적인 제한은 없지만 return에만 쓰는것이 좋다  
}
```
- 문서
	![](../img/Pasted%20image%2020220921075436.png)
### 2. Optionnl.of( ) -> of 내에는 무조건 null이 아닌 경우에 사용
- 문서
	![](../img/Pasted%20image%2020220921075319.png)



## 주의할 것
### 1. 리턴값으로만 쓰기 (메소드 매개변수 타입, Map의 키 타입, 인스턴스 field 타입으로 쓰지 말자)
- 매개변수 타입으로 쓸 수는 있지만 쓰지 않는다.  null이 들어갈 수 있기 때문에 체크를 또 해줘야 한다 (`옵셔널을 쓰는 의미가 없음. 오히려 더 번거로움`)
```java
  public void setProgress ( Optional<Progress> progress ) {  
            progress.ifPresent( p -> this.progress = p );  
    }
```
이렇게 하고 null을 셋팅하면 NPE가 여전히 발생한다.
![](../img/Pasted%20image%2020220921080832.png)

null체크를 위해 코드를 추가할때 인텔리제이가 추천하는대로 progress.isPresent()를 쓰면 이 자체로 NPE가 발생한다.. 쓰지말자!
```java
public void setProgress ( Optional<Progress> progress ) {  
    // if ( progress!= null ) 이렇게 쓰면 인텔리제이가 Optional의 메소드 쓰라고 한다
    if ( progress.isPresent() ) {  // <- progress가 null이라서 이 자체로 NPE발생!
        progress.ifPresent( p -> this.progress = p );  
    }  
}
```


+) 이제는 인텔리제이가 먼저 주의를 준다..!!!
![](../img/Pasted%20image%2020220921080348.png)




### 2. Optional을 리턴하는 메소드에서 null을 리턴하지 말자
- 리턴하는 메소드에서 이렇게 null을 리턴하는 경우,
	```java
	public Optional<Progress> getProgress () {  
	    return null;  
	    //return Optional.ofNullable( progress ); 
	}
	```
	클라이언트쪽에서 Optional이 제공하는 메소드인 ifPresent()를 써봤자 null을 리턴받기 때문에 NPE가 발생한다
	![](Pasted%20image%2020220922070302.png|450)
- 따라서 없다는 것을 표현하려면 리턴하는 메소드에서 Optional.empty()을 리턴하자
	```java
	public Optional<Progress> getProgress () {  
	    return Optional.empty();  
	}
	```

### 3. primitive타입은 primitive용 Optional을 따로 쓰자
- 사용하지 않으면 내부적으로 박싱/언박싱이 빈번하게 일어나므로 성능상 좋지 않다.
	```java
	/* primitive 타입을 그냥 넣으면 내부적으로 Boxing/Unboxing 이 일어나서 성능에 안좋다. */  
	Optional.of(10);  
	OptionalInt.of( 10 );
	```

### 4. Collection, Map, Stream Array, Optional은 Optional로 감싸지 말 것.
- 이미 그 자체로 비어있는지 판단할 수 있는 컨테이너들을 Optional로 감싸는 것은 **Optional을 두 번 감싸는 것과 같다.**


## Optional API 사용하기

### 준비
```java
List<OnlineClass> springClasses = new ArrayList<>();  
springClasses.add(new OnlineClass(1, "spring boot", true));   
springClasses.add(new OnlineClass(5, "rest api development", false));
```

### Optional 만들기


### Optional에 값이 있는지 없는지 확인하기


### Optional에 있는 값 가져오기


### Optional에 값이 있는 경우에 그 값을 가지고 xxx를 하라.


### Optional에 값이 있으면 가져오고 없으면 xxx를 리턴하라


### Optional에 값이 있으면 가져오고 없으면 xxx를 하라


### Optional에 값이 있으면 가져오고 없으면 에러를 던져라


### Optional에 들어있는 값 걸러내기


### Optional에 들어있는 값 변환하기













# 출처(참고문헌)
- 더 자바, Java 8 , Optional 소개
- 이펙티브 자바 3판, 아이템 55 적절한 경우 Optional을 리턴하라
- [Class Optional<>](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html)
- [Tired of Null Pointer Exceptions? Consider Using Java SE 8's `"Optional"!`](https://www.oracle.com/technical-resources/articles/java/java8-optional.html)

# 연결문서
- [자바가 변화하는 이유](자바가%20변화하는%20이유.md)
