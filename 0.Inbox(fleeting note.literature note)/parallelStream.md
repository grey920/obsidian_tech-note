# 날짜 : 2022-09-19 22:04

# 주제 : 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```

## parallelStream
- Stream을 병렬적으로 받아서 처리한다
- 병렬 처리이기 때문에 순서를 보장하지 않는다. 
- 별도의 설정이 없으면 해당 어플리케이션이 구동되는 스펙에 따라 쓰레드 수가 결정된다. 
	- 쓰레드가 N개 생성되었을 때, 하나는 main 쓰레드로 스트림을 처리하는 기본 쓰레드, 나머지 N - 1개의 쓰레드가 ForkJoinPool 쓰레드이다. 
- 여기서 사용하는 ForkJoinPool을 Custom Pool을 별도로 생성해서 활용하지 않으면, JVM 인스턴스에서 공통적으로 사용하는 Pool을 사용한다



## 예시 코드
```java
/**  
 * parallelStream()을 사용하여 병렬 처리. ( ForkJoinPool을 써서 다른 쓰레드 사용 )  
 */
 List<String> collect = names.parallelStream().map( s -> {  
    System.out.println( s + " " + Thread.currentThread().getName() );  
    return s.toUpperCase();  
} ).collect( Collectors.toList() );  
  
```
이를 실행해보면 [ForkJoinPool](ForkJoinPool.md)을 써서 각자 다른 쓰레드가 출력됨을 볼 수 있다.
```
이미자 ForkJoinPool.commonPool-worker-3
정겨운 ForkJoinPool.commonPool-worker-7
정중기 main
정다와 ForkJoinPool.commonPool-worker-5
```

일반 stream()을 사용하면 모두 같은 main 쓰레드를 사용한다
```java
    List<String> collect = names.stream().map( s -> {  
        System.out.println( s + " " + Thread.currentThread().getName() );  
        return s.toUpperCase();  
    } ).collect( Collectors.toList() );  
  
    collect.forEach( System.out::println );  
  
}

>>
>>정겨운 main
>>정다와 main
>>정중기 main
>>이미자 main
```


## parallelStream의 비용
- 병렬 처리가 모두 빠르고 좋은 것은 아니다. 여기에도 비용이 들기 때문에 오히려 더 느려질 수 있다.
- 쓰레드를 만들고 병렬적으로 처리한 것들을 수집할 떄의 overhead나 쓰레드를 오고가는 컨텍스트 스위칭 비용 등을 합하면 더 느릴 수도 있다.


### parallelStream이 유용한 경우는??
- **데이터가 아주 방대하게 큰 경우**. 이런 경우가 아니라면 대부분의 경우, 한 쓰레드에서 하는 것이 낫다. 
- 스트림 안에서 처리하는 내용에 따라 다를 수 있기 때문에, 성능 측정해서 실제 어느것이 효과적인지 테스트 해보는 것이 좋다. 



# 출처(참고문헌)
- 더 자바 8
- [Java의 Stream에서 parallelStream은 stream보다 항상 빠를까?](https://twinparadox.tistory.com/627)
- [반복문 vs stream vs parallelStream 중 뭐가 제일 빠를까?](https://duck67.tistory.com/30)

# 연결문서
- [스트림 API](스트림%20API.md)
