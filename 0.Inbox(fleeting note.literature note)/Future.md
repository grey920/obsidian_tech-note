# 날짜 : 2022-10-02 21:28

# 주제 : #java #concurrent프로그래밍 #thread 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```



## 결과 가져오기 - get()
```java
ExecutorService executorService = Executors.newSingleThreadExecutor();
Future<String> helloFuture = executorService.submit(() -> {

    Thread.sleep(2000L);

    return "Callable";
});

System.out.println("Hello");

String result = helloFuture.get();

System.out.println(result);
executorService.shutdown();
```

```ad-note
get()과 join()은 유사하지만 차이점이라면, join()은 에러 발생시 UnCheckedException이 발생하고 get()은 CheckedException이 발생한다. 
```


## 작업 상태를 확인하기 - isDone()
### isDone()
```java
// Single Thread로 ExecutorService 생성  
ExecutorService executorService = Executors.newSingleThreadExecutor();  
  
// Callable 생성(정의)  
Callable<String> hello = () -> {  
    Thread.sleep( 2000L );  
    return "Hello";  
};  
  
// Callable 사용 ( Runnable과 같다 )
Future<String> helloFuture = executorService.submit( hello );  
System.out.println( helloFuture.isDone() );  
System.out.println("Started!!!!!!!!!!!!!!!!");  
  
  
// submit이 만들어주는 값을 받아옴  
// [주의!] 값을 받을때까지 기다린다 -> 그냥 무작정 기다리기만 해야하나?  
// ==> 상태를 알 수 있다 ( isDone()으로 체크 )
helloFuture.get(); // Blocking Call  
  
System.out.println( helloFuture.isDone() );  
System.out.println("END!!!!!!!!!!!!!!!!!!!!");  
// task들 종료  
executorService.shutdown();

>> false
>> Started!!!!!!!!!!!!!!!!
>> true
>> END!!!!!!!!!!!!!!!!!!!!
```
- task에 Thread.sleep()으로 2초의 딜레이를 주었기 때문에 Future.get()을 하면 결과값을 받을 때까지 무작정 계속 기다리게 된다.
- 이때 isDone()으로 작업 상태를 확인할 수 있다. 완료했으면 true 아니면 false를 리턴한다.


## 작업 취소하기 - cancel()

```java
System.out.println( helloFuture.isDone() );
System.out.println("Started!!!!!!!!!!!!!!!!");  

// 작업 취소하기  
// true : 현재 진행중인 작업을 interrupt하면서 종료시킨다.  
// false: 기다린다. 하지만 일단 cancel을 호출하면 후에 get()으로 값을 가져올 수 없다. 
helloFuture.cancel( false );  

// 작업을 종료시켰기 때문에 isDone은 무조건 true
System.out.println( helloFuture.isDone() );  

// [ERROR] 취소한 작업에 값을 가져오라고 하면 java.util.concurrent.CancellationException 에러가 발생함.  
helloFuture.get();  
System.out.println("END!!!!!!!!!!!!!!!!!!!!");  

// task들 종료  
executorService.shutdown();

>> false
>> Started!!!!!!!!!!!!!!!!
>> true
Exception in thread "main" java.util.concurrent.CancellationException
	at java.base/java.util.concurrent.FutureTask.report(FutureTask.java:121)
	at java.base/java.util.concurrent.FutureTask.get(FutureTask.java:191)
	at me.whiteship.java8to11.FutureApp.main(FutureApp.java:35)
```



## 여러 작업 동시에 실행하기 - invokeAll()
```java
public static void main ( String[] args ) throws ExecutionException, InterruptedException {  
    // Single Thread로 ExecutorService 생성  
    ExecutorService executorService = Executors.newSingleThreadExecutor();  
  
    // 여러개의 작업 정의  
    Callable<String> hello = () -> {  
        Thread.sleep( 2000L );  
        return "Hello";  
    };  
  
    Callable<String> java = () -> {  
        Thread.sleep( 3000L );  
        return "Java";  
    };  
  
    Callable<String> grey = () -> {  
        Thread.sleep( 1000L );  
        return "Grey";  
    };  
  
  
    // Collection을 만들어 한꺼번에 보내기  
    // invokeAll()은 모든 작업이 끝날때까지 기다렸다가 한꺼번에 결과물을 보내준다  
    List<Future<String>> futures = executorService.invokeAll( Arrays.asList( hello, java, grey ) );  
    for( Future<String> f : futures ){  
        System.out.println( f.get() );  
    }  
  
    // task들 종료  
    executorService.shutdown();  
}
```
- 예) 여러 주가 정보를 가져와서 내가 현재 보유한 주가가 얼마인지 조회할 때


## 여러 작업 중에 하나라도 먼저 응답이 오면 끝내기 - invokeAny()
```java
 public static void main ( String[] args ) throws ExecutionException, InterruptedException {  
        // Single Thread로 ExecutorService 생성  
//        ExecutorService executorService = Executors.newSingleThreadExecutor();  
        ExecutorService executorService = Executors.newFixedThreadPool( 2 );  
  
        // 여러개의 작업 정의  
        Callable<String> hello = () -> {  
            Thread.sleep( 4000L );  
            return "Hello";  
        };  
  
        Callable<String> java = () -> {  
            Thread.sleep( 3000L );  
            return "Java";  
        };  
  
        Callable<String> grey = () -> {  
            Thread.sleep( 1000L );  
            return "Grey";  
        };  
  

        String s = executorService.invokeAny( Arrays.asList( hello, java, grey ) );  
        System.out.println( "s = " + s );  
  
        // task들 종료  
        executorService.shutdown();  
    }
```
- 예) 백업용으로 하나의 파일을 3 대의 서버에 복사해서 보관한 상황. 이 때 해당 파일을 조회하는 경우 굳이 세 대의 서버에서 모두 줄 때까지 기다릴 필요가 없다. 가장 빨리 오는 것 하나만 받으면 되기 때문에 invokeAny()를 사용한다.
```ad-attention
invokeAny()에 Collection으로 `쓰레드 갯수가 태스크 수만큼 있지 않다면`, 넘겨줄 때 그 **순서**가 중요한 것 같다..!!

강의에서는 싱글쓰레드로 했을 때 hello 태스크에 4초의 딜레이가 걸려있음에도 계속 hello가 출력되었다. 
테스트로 Arrays.asList()의 인자 순서를 java, hello, grey 순으로 바꿨더니 이젠 계속 java만 출력되었다..!!!!!
- 멀티쓰레드로 쓰레드2개를 만들어 테스트를 했을 때에도 역시 순서대로 태스크가 할당되는 듯 했다. hello, java, grey 순으로 넘기면 4초 딜레이인 hello와 3초 딜레이인 java 중 더 빨리 종료되는 java를 값으로 리턴했다. 1초인 grey 태스크는 쓰레드에 순서에 밀려 쓰레드에 할당되지 않아서 아예 선택권 자체에 없는 듯 하다.
- 예상대로 쓰레드를 3개로 늘리면 grey가 리턴된다.
- 쓰레드를 3개로 늘리고 java와 grey 둘 다 동일하게 2초로 딜레이를 주었더니 Java와 Grey가 번갈아 출력된다. 

즉, invokeAny()는 가장 먼저 값이 리턴되는 것을 꺼내주므로, 쓰레드 수가 task수보다 작다면 쓰레드 수만큼의 task가 순서대로 짤려서 실행되고 그 중 빨리 종료되는 task의 값이 리턴되는 것이다
- 그래서 싱글 쓰레드이면 가장 처음 들어온 task에서 짤리기 때문에 아무리 실행해도 첫번째 task의 값만 출력되는 것 같다. 
```




# 출처(참고문헌)
- 

# 연결문서
-  [Callable](Callable.md)
- [CompletableFuture](CompletableFuture.md)
