# 날짜 : 2022-09-14 08:01

# 주제 : #java8 #인터페이스
----
# 메모
```ad-note
title: tl;dr
- 인터페이스에 메소드 선언이 아니라 구현체를 제공하는 방법. 
- 인터페이스를 구현한 클래스를 깨트리지 않고 인터페이스에 새 기능을 추가할 수 있다
```

```toc
```


## 왜 생겼을까?
- 원래 interface를 구현하면 기본으로 **인터페이스의 모든 메서드를 구현체에서 구현해야만 했다.** -> 추후에 인터페이스에 메서드를 추가하면 이를 구현한 구현체들 모두에서 `컴파일 에러`가 발생!! 

- 인터페이스의 메서드에 **default 키워드**를 붙이면 구현체에서 직접적으로 구현하지 않고도 인터페이스에서 구현한 내용을 자동으로 구현체가 해당 기능을 가지게 된다. 

- 즉, 인터페이스를 구현한 클래스들을 깨트리지 않고 인터페이스에 새 기능을 추가할 수 있다. 


## 주의할 점
- default 메소드는 구현체가 모르게 추가된 기능이기에 그만큼 `리스크`가 있다. 
	- 컴파일 에러는 아니지만 구현체에 따라 런타임 에러가 발생할 수 있다. 
	- 1. `@implSpec` 자바독 태그를 사용해서 반드시 문서화 할 것!
	- 2. 문제가 발생한다면 구현체에서 `재정의`를 할 수도 있다.

- Object가 제공하는 메서드(equals, hashCode)는 default 메소드로 제공할 수 없다. 
	- 이는 구현체가 재정의해야 한다. 

- 본인이 수정할 수 있는 인터페이스에만 기본 메소드를 제공할 수 있다.


## 방법

### 인터페이스를 상속받는 인터페이스에서 다시 추상메소드로 변경할 수 있다.


![](../img/Pasted%20image%2020220916074415.png)
- 인터페이스 Foo와 Bar 모두 같은 default 메서드를 가지고 있는 경우 구현체인 DefaultFoo는 어떤 것을 써야할 지 모르기 때문에 컴파일 에러가 난다.
- 이렇게 충돌하는 경우에는 직접 오버라이딩을 해야한다.
	![](../img/Pasted%20image%2020220916074955.png)
	```java
	package me.whiteship.java8to11;  
	  
	public class DefaultFoo implements  Foo, Bar {  
	  
	    public String name;  
	  
	    public DefaultFoo ( String name ) {  
	        this.name = name;  
	    }  
	  
	  
	    @Override  
	    public void printName () {  
	        System.out.println( this.name );  
	    }  
	  
	    /**  
	     * @implSpec getName()으로 리턴받은 문자열을 대문자로 변환하여 출력한다.  
	     */    @Override  
	    public void printNameUpperCase () {  
	        Foo.super.printNameUpperCase(); // 여기서 Bar로 할지 Foo로 할지 직접 오버라이딩 해줘야 한다  
	    }  
	  
	    @Override  
	    public String getName () {  
	        return this.name;  
	    }  
	}
	```







# 출처(참고문헌)
- 

# 연결문서
- [자바8의 인터페이스 변화](자바8의%20인터페이스%20변화.md) 
