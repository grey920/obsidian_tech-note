# 날짜 : 2022-09-28 06:45

# 주제 : #java #병렬성 #concurrent프로그래밍
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```


```ad-question
title: Concurrent 소프트웨어?
**동시에 여러 작업**을 할 수 있는 소프트웨어. 
- 예를 들어, 웹 브라우저로 인프런을 보면서 인텔리제이로 코드를 치고 동시에 옵시디언으로 노트 정리를 할 수 있다.
- 한 애플리케이션에서도 동시에 여러 작업이 일어나는 것도 concurrent 프로그래밍이다.
```


## 자바에서 지원하는 Concurrent 프로그래밍
### 멀티 프로세싱 - ProcessBuilder
- ProcessBuilder를 사용해서 한 프로세스에서 다른 프로세스를 생성하는 것이 가능하다. 

### 멀티쓰레드



## 자바에서 쓰레드를 구현하는 2가지 방법 

### 1. Thread를 상속(extends)받아 구현 - 불편한 방법
```java
public class MultiThreadApp {  
    public static void main ( String[] args ) {  
        // application이 동작하는 main thread
        System.out.println( Thread.currentThread().getName() );  
  
        /* main thread에서 다른 thread를 생성하는 방법  
        1. Thread를 상속받아서 구현 - 불편한 방법  
        * */        
        MyThread myThread = new MyThread();  
        myThread.start(); // 쓰레드 실행  
  
        System.out.println( "Hello::: " + Thread.currentThread().getName() );  
  
  
    }  
  
    /**  
     * 1. Thread 상속받아 구현.  
     * run()을 구현  
     */  
    static class MyThread extends Thread {  
        @Override  
        public void run () {  
            System.out.println( "Thread::: " + Thread.currentThread().getName() );  
        }  
    }  
}

>> main
>> Hello::: main
>> Thread::: Thread-0
```
- 쓰레드의 순서를 보장받을 수 없다. 코드로 보면 구현한 쓰레드 내의 'Thread::: Thread-0'이 먼저 출력될 것 같지만 실제로는 실행할때마다 다르다.


### 2. Thread의 생성자에 Runnable을 주는 방법
```java
public class MultiThreadApp {  
    public static void main ( String[] args ) {  
  
        /* main thread에서 다른 thread를 생성하는 방법  
        2. Thread의 생성자에 Runnable을 주는 방법  
        * */        
        Thread thread = new Thread( new Runnable() {  
            @Override  
            public void run () {  
                System.out.println( "Thread::: " + Thread.currentThread().getName() );  
            }  
        } );  
        thread.start();
  
        System.out.println( "Hello::: " + Thread.currentThread().getName() );  
  
    }  
  
}

>> Hello::: main
>> Thread::: Thread-0
        
```
- Runnable이 [함수형 인터페이스 (Functional Interface)](함수형%20인터페이스%20(Functional%20Interface).md)이기 때문에 자바8 이후에는 람다로 구현할 수 있다.
	```java
	Thread thread = new Thread( () -> System.out.println( "Thread::: " + Thread.currentThread().getName() ) );  
	thread.start();
	```



## 쓰레드의 주요한 기능 세 가지 
### 1. 현재 쓰레드를 대기시키기: Thread.sleep()
- 쓰레드를 대기시키면 굳이 자고있는 쓰레드에 리소스를 할당할 필요가 없으므로 `다른 쓰레드에게 우선권이 넘어간다.`
	- 우선권이 부여되기 때문에 거의 대부분 Hello::: main 쓰레드가 먼저 출력된다.
```java
public static void main ( String[] args ) {  
  
    Thread thread = new Thread( () -> {  
        try {  
            Thread.sleep( 1000L );  
        }  
        catch ( InterruptedException e ) {  
            throw new RuntimeException( e );  
        }  
  
        System.out.println( "Thread::: " + Thread.currentThread().getName() );  
    });  
    thread.start();  
  
    System.out.println( "Hello::: " + Thread.currentThread().getName() );  
  
}

>> Hello::: main
>> Thread::: Thread-0
```
- InterruptedException : 자는 동안에 누군가가 쓰레드를 깨우면 이 익셉션 안으로 들어온다. 
	- 깨우는 방법?? -> interrupt

### 2. 다른 쓰레드를 깨우기: Thread.interrupt
```java
public class MultiThreadApp {  
    public static void main ( String[] args ) throws InterruptedException {  
  
        Thread thread = new Thread( () -> {  
            // 무한루프 생성  
            while(true){  
                System.out.println( "Thread::: " + Thread.currentThread().getName() );  
                try {  
                    Thread.sleep( 1000L );  
                }  
                catch ( InterruptedException e ) {  
                    System.out.println("exit!!!!");  
                    return;  
                }  
            }  
        });  
        thread.start();  
  
        System.out.println( "Hello::: " + Thread.currentThread().getName() );  
        Thread.sleep( 3000L );  
        thread.interrupt();  
  
    }  
  
}

>> Thread::: Thread-0
>> Hello::: main
>> Thread::: Thread-0
>> Thread::: Thread-0
>> exit!!!!
```
- **Thread.interrupt()** 는 `InterruptedException`을 발생시키는 역할만 한다 -> 해당 쓰레드를 종료시키려면 InterruptedException을 잡는 catch문에서 `return`으로 종료시켜야 한다.
	- return하지 않으면 해당 쓰레드는 계속 작업을 진행한다.
	```java
	catch ( InterruptedException e ) {  
                    System.out.println("exit!!!!");  
	//                    return;  
     }
    >> Hello::: main
	>> Thread::: Thread-0
	>> Thread::: Thread-0
	>> Thread::: Thread-0
	>> exit!!!!
	>> Thread::: Thread-0
	>> Thread::: Thread-0
	>> Thread::: Thread-0
	```

### 3. 다른 쓰레드를 기다리기: join()
- 기다릴 쓰레드에 join을 건다.
```java
public class MultiThreadApp {  
    public static void main ( String[] args ) throws InterruptedException {  
  
        Thread thread = new Thread( () -> {  
                System.out.println( "Thread::: " + Thread.currentThread().getName() );  
                try {  
                    Thread.sleep( 3000L );  
                }  
                catch ( InterruptedException e ) {  
                    throw new IllegalArgumentException( e );  
                }  
        });  
        thread.start();  
  
        System.out.println( "Hello::: " + Thread.currentThread().getName() );  
        thread.join(); // main 쓰레드가 thread가 끝날때까지 기다린다. 기다리지않으면 'thread is finished'는 아무때나 출력된다.  
        System.out.println( thread+ " is finished");  
  
    }  
  
}

>> Thread::: Thread-0
>> Hello::: main
>> Thread[Thread-0,5,] is finished
```
- thread.join()을 걸면 메인 쓰레드가 thread의 3초를 기다렸다가 다음 라인인 `Thread[Thread-0,5,] is finished`를 출력하고 종료된다.
- 하지만 thread.join()을 주석하면 `Thread[Thread-0,5,] is finished`가 바로 출력되고 계속 돌다가 종료된다.
- 대기하는 도중에 누군가 main 쓰레드를 interrupt하면 역시 InterruptedException이 일어난다
**=> 복잡하다!!! (문제임) => 개발자가 직접 쓰레드를 코딩으로 관리하면 안되기 때문에 Executor가 생김 => Executor를 사용하면 Future를 사용할 수 있게된다.**


# 출처(참고문헌)
- 

# 연결문서
- 
