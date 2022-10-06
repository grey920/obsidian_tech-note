# 날짜 : 2022-10-03 15:44

# 주제 : #java8 #thread #concurrent프로그래밍 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```

## CompletableFuture 
자바에서 비동기(Asynchronous) 프로그래밍을 가능하게 하는 인터페이스.
- [Future](Future.md)를 사용해서도 어느정도 가능했지만 ***하기 힘든 일***들이 많았다
- 명시적으로 Executor를 쓰지 않아도 된다! -> 왜? CompletableFuture만으로도 비동기 처리를 할 수 있기 때문에!

```ad-info
title: Complete가 붙은 이유
외부에서 명시적으로 `complete` 시킬 수 있다
- 예) 몇 초 이내에 응답값이 안오면 특정 값으로 리턴하도록 만든다
```


### Future로는 하기 힘들었던 일??
- 콜백을 줄 수 없기 때문에 Future만으로는 어떤 일을 이어서 처리하기는 힘들었다.
	- 예) 비동기적인 작업 두 개를 이어서 처리하기



# 출처(참고문헌)
- 

# 연결문서
- 
