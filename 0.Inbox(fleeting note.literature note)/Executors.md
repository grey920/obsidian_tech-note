# 날짜 : 2022-09-29 14:35

# 주제 : #java #concurrent프로그래밍 #thread #task
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 자바 애플리케이션에서 실행되는 task를 간단하게 비동기로 처리할 수 있는 쓰레드풀과 API를 사용할 수 있는 JDK에서 제공하는 Framework
```


## Executors가 왜 필요했나
- 자바에서 멀티쓰레드 작업을 할 때 개발자가 직접 쓰레드를 관리하는 것은 매우 어렵고 불편하다. 
	![](자바의%20Concurrent%20프로그래밍#^b782e7)
- 따라서 이러한 **쓰레드 관리의 어려움**을 해결하기 위해 `쓰레드를 생성하고 생명주기를 관리하며 작업 처리 및 실행을 위임`할 Executors가 등장하게 되었다!


## Executors가 하는 일
### 쓰레드 만들기
- 애플리케이션이 사용할 [쓰레드풀 (Thread Pool)](쓰레드풀%20(Thread%20Pool).md)을 만들어 관리
### 쓰레드 관리
- 쓰레드의 생명주기를 관리
### 작업 처리 및 실행
- 쓰레드로 실행할 작업을 제공할 수 있는 API를 제공
- [[Callable과 Runnable]] . 쓰레드는 Runnable과 Callable 인터페이스의 구현된 함수를 수행한다.
	- 즉, Thread를 만들때 Runnable또는 Callable의 구현체를 넘겨서 실행할 작업을 제공한다.
	- Runnable은 값을 리턴하지 않고 Exception을 발생시키지도 않는다.
	- Callable은 특정 타입의 객체를 리턴하고 Exception을 발생시킬 수 있다.

```ad-attention
- 개발자 : Runnable 안에 task를 작성하여 그 Runnable만 Executors에게 넘긴다.
- Executors : Thread 생성, 종료 등의 일련의 작업을 한다.
	- Executors의 주요 인터페이스 중 실질적으로는 ExecutorService를 사용한다.
```


## Executors의 주요 인터페이스 
java.util.concurrent 패키지에 정의된 세 가지 executor 인터페이스

### Executor 인터페이스
- 사실 이 타입을 쓸 일은 거의 없다. 
- 안에 execute() 메서드 하나밖에 없다.
	![](../img/Pasted%20image%2020221002201522.png)

### [ExecutorService 인터페이스](ExecutorService%20인터페이스.md)
- Executor를 상속받은 인터페이스. 실질적으로 사용하는 인터페이스로 Executors 클래스의 정적 팩토리 메서드를 사용하여 구현 객체를 만들 수 있다.
	- Executors.newSingleThreadExecutor(), Executors.newFixedThreadPool( 3 ), ...
- Executor보다 더 다양한 메서드들이 있다. 
	- submit( Callable 또는 Runnable ) : 작업 처리
	- shutdown() : 작업 종료
	```java
	// Thread 하나만 생성
	ExecutorService executor = Executors.newSingleThreadExecutor();
	executorService.submit( () -> {  
    System.out.println( "Thread ::: " + Thread.currentThread().getName() );  
	} );
	```

	```java
	// Thread 여러개 생성
	ExecutorService executorService = Executors.newFixedThreadPool( 3 );  
	  
	executorService.submit( getRunnable( "Hello" ) );  
	executorService.submit( getRunnable( "Grey" ) );  
	executorService.submit( getRunnable( "The" ) );  
	executorService.submit( getRunnable( "Java" ) );  
	executorService.submit( getRunnable( "Thread" ) );
	```

	Runnable
	```java
	/**  
	 * Runnable은 void이다. 리턴을 받으려면 Callable을 사용해야 한다.  
	 * @param msg  
	 * @return  
	 */  
	private static Runnable getRunnable ( String msg ) {  
	    return () -> System.out.println( msg + Thread.currentThread().getName());  
	}
	```

- main 애플리케이션에서 ExecutorService로 task들을 보내면 -> ExecutorService 내에 있는 [쓰레드풀 (Thread Pool)](쓰레드풀%20(Thread%20Pool).md)에 쓰레드가 있어서 task가 수행되고, 만약 쓰레드가 다 차있으면 BlockingQueue에 남은 task들을 쌓아놓는다. 쓰레드 하나가 작업을 마치면 BlockingQueue에서 기다리던 task가 들어간다.
- [쓰레드풀 (Thread Pool)](쓰레드풀%20(Thread%20Pool).md)을 쓰면 쓰레드 생성 비용이 줄어든다는 장점이 있다. 



### ScheduledExecutorService 인터페이스
- ExecutorService를 상속받은 인터페이스
- schedule(), scheduleAtFixedRate() 등의 메서드를 통해 일정 시간의 딜레이를 준 후 작업을 실행하거나 주기적으로 어떤 작업을 실행하고 싶을 때 사용한다.
```java
ScheduledExecutorService executorService = Executors.newSingleThreadScheduledExecutor();

// 스케쥴을 받아서 3초 기다린 후 실행  
executorService.schedule( getRunnable("Hello"), 3, TimeUnit.SECONDS );  
executorService.shutdown();

>> ( 3초 후 )
>> Hellopool-1-thread-1
```

```java
ScheduledExecutorService executorService = Executors.newSingleThreadScheduledExecutor();

// 반복 작업을 하는 경우 ( 1초 기다렸다가 2초에 한 번씩 출력)  
// shutdown()을 하면 안에 task가 InterruptedException을 받아 그 때 종료가 되어버리기 때문에 shutdown()을 주석해야 확인 가능.  
executorService.scheduleAtFixedRate( getRunnable("Hello"), 1, 2,  TimeUnit.SECONDS );
```
- scheduleAtFixedRate()에 [Callable](Callable.md) 타입을 주면 [Future](Future.md) 타입으로 리턴값을 받을 수 있다.


# 출처(참고문헌)
 - https://docs.oracle.com/javase/tutorial/essential/concurrency/executors.html
 - 

# 연결문서
- [자바의 Concurrent 프로그래밍](자바의%20Concurrent%20프로그래밍.md)
- [쓰레드풀 (Thread Pool)](쓰레드풀%20(Thread%20Pool).md)
- [ForkJoinPool](ForkJoinPool.md)
- [CompletableFuture](CompletableFuture.md)