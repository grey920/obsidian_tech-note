# 날짜 : 2022-10-27 06:35

# 주제 : #proxy 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 매번 프록시에 해당하는 클래스를 만드는 것이 아니라 동적으로 런타임에 인터페이스 또는 클래스의 프록시 인스턴스 또는 클래스를 생성해내는 방법.
```


```ad-note
다이나믹 프록시도 [리플렉션](리플렉션.md)의 일부이다.
 -> java.lang.reflect 패키지 안에 들어있다.
```



## 다이나믹 프록시가 나온 배경
- 프록시 패턴에서 보면 프록시를 적용하기 위해서는 프록시 클래스를 기능마다 일일히 다 만들어주어야 했다.
- 이를 수백 수천개 만들지 않고 동적으로 런타임때 만들 수 있다면 깔끔하고 좋지 않을까?


## 다이나믹 프록시 적용하기
- 클래스를 따로 만들지 않고, java.lang.reflect의 Proxy 객체를 사용한다.

### 1. 프록시 객체 생성
Proxy.newProxyInstance()로 프록시 인스턴스를 만든다. 
```java
// OLD - proxy pattern using proxy class
// BookService bookService = new BookServiceProxy( new DefaultBookService() );

// NOW
BookService bookService = ( BookService ) Proxy.newProxyInstance()
```
newProxyInstance()는 3개의 인자를 받는다. 
1. `클래스로더` : 어떤 클래스로더를 써서 만들것인지 ( 예. BookService.class를 읽어들인 로더)
2. `인터페이스 목록(서브젝트)` : 이 프록시가 어떤 인터페이스의 구현체인지
3. `InvocationHandler` : 이 프록시의 어떤 메서드가 호출될 때, 그 메서드 호출을 어떻게 처리할 것인지에 대한 설명
	- -> 리얼 서브젝트가 여기 안에 있어야 한다

```java
BookService bookService = ( BookService ) Proxy.newProxyInstance( BookService.class.getClassLoader(), new Class[]{ BookService.class }, new InvocationHandler() {  
    BookService bookService = new DefaultBookService();  
    @Override  
    public Object invoke ( Object proxy, Method method, Object[] args ) throws Throwable {  
      return null;  
    }  
} );
```


### 2. InvocationHandler 에 부가기능을 명시한다
```java
BookService bookService = ( BookService ) Proxy.newProxyInstance( BookService.class.getClassLoader(), new Class[]{ BookService.class }, new InvocationHandler() {  

    BookService bookService = new DefaultBookService();  
    @Override  
    public Object invoke ( Object proxy, Method method, Object[] args ) throws Throwable {  
    
        // 만약 메소드가 엄청 많고 그 중에 몇몇개만 적용하고 싶다면?? -> 여기에서 분기처리를 다 해주거나 InvocationHandler를 감싸는 프록시를 또 만들거나.. InvocationHandler가 유연하지 못하다  
        // => 이러한 단점때문에 스프링에서 나온 기술이 AOP (프록시 기반의 AOP)  
        /* rent()만 적용하고 나머지 메서드들은 그대로 반환한다 */        
        if ( method.getName().equals( "rent" ) ) {  
            System.out.println("aaaaaaaa");  
            Object invoke = method.invoke( bookService, args );  
            System.out.println("aaaaaaaa");  
            return invoke;  
        }  
  
        return method.invoke( bookService, args );  
  
    }  
} );
```

```
결과
aaaaaaaa
book.getTitle() = spring
aaaaaaaa
return: spring
```
이제 프록시 클래스 필요없이 동적으로 프록시가 적용된다. 
하지만 InvocationHandler는 유연하지 못해서 특정 메서드에 다르게 적용해야 할 경우 위의 예시처럼 다 **분기처리**를 해줘야 한다. 또는 InvocationHandler를 감싸는 프록시를 또 만들어야 한다.. 
이러한 단점때문에 나온 기술이 스프링의 AOP이다. 



## JDK 다이나믹 프록시의 제약사항
인터페이스에만 적용된다. 클래스에는 쓸 수가 없다.
따라서 만약 BookService 인터페이스가 아니라 DefaultBookService 클래스였다면 InvocationHandler를 사용할 수 없다
```java
DefaultBookService bookService = ( DefaultBookService ) Proxy.newProxyInstance( BookService.class.getClassLoader(), new Class[]{ DefaultBookService.class }, new InvocationHandler() {  

    DefaultBookService bookService = new DefaultBookService();  
    
    @Override  
    public Object invoke ( Object proxy, Method method, Object[] args ) throws Throwable {  
        if ( method.getName().equals( "rent" ) ) {  
            System.out.println("aaaaaaaa");  
            Object invoke = method.invoke( bookService, args );  
            System.out.println("aaaaaaaa");  
            return invoke;  
        }  
  
        return method.invoke( bookService, args );  
  
    }  
} );
```

```
결과
java.lang.IllegalArgumentException: com.example.proxy.DefaultBookService is not an interface
```

클래스의 프록시가 필요한 경우에는 CGLib나 ByteBuddy를 사용하는 방법이 있다. 


## CGLib와 ByteBuddy의 제약사항
- subclass를 사용하는 방법이기때문에 타겟 클래스가 상속을 허용하지 않는 경우 이 방법을 사용할 수 없다.
	- final 클래스이거나 생성자가 private default만 있는 경우



## 다이나믹 프록시 사용처
- 스프링 데이터 JPA
- 스프링 AOP
- Mockito
- 하이버네이트 lazy initialization
- ...



# 출처(참고문헌)
- [인프런] 더 자바, 코드를 조작하는 다양한 방법, 백기선

# 연결문서
- [리플렉션](리플렉션.md)
