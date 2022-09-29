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


## Executors가 하는 일
### 쓰레드 만들기
- 애플리케이션이 사용할 [쓰레드풀 (Thread Pool)](쓰레드풀%20(Thread%20Pool).md)을 만들어 관리
### 쓰레드 관리
- 쓰레드의 생명주기를 관리
### 작업 처리 및 실행
- 쓰레드로 실행할 작업을 제공할 수 있는 API를 제공



## Executors의 주요 인터페이스 
java.util.concurrent 패키지에 정의된 세 가지 executor 인터페이스

### Executor 인터페이스

### [[ExecutorService 인터페이스]]

### ScheduledExecutorService 인터페이스



# 출처(참고문헌)
 - https://docs.oracle.com/javase/tutorial/essential/concurrency/executors.html
 - 

# 연결문서
- [자바의 Concurrent 프로그래밍](자바의%20Concurrent%20프로그래밍.md)
- [쓰레드풀 (Thread Pool)](쓰레드풀%20(Thread%20Pool).md)
- [ForkJoinPool](ForkJoinPool.md)
