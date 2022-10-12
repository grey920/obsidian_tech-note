# 날짜 : 2022-10-13 06:58

# 주제 : #java8 #병렬성 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- Arrays.parallelSort() : 정렬할 때 [ForkJoinPool](ForkJoinPool.md) 프레임워크를 사용해서 배열을 병렬로 정렬하는 기능을 제공한다
- 알고리즘의 효율은 기존과 같지만 분산처리로 좀 더 빠르게 정렬이 가능하다
```


## 병렬 정렬 알고리즘
- [ForkJoinPool](ForkJoinPool.md)에서 제공받은 여러 쓰레드에 분산해서 아래의 작업을 처리한다
- 1. 배열을 둘로 계속 쪼갠다
- 2. 합치면서 정렬한다

```java
public static void main ( String[] args ) {  
  
    int size = 1500;  
    int[] numbers = new int[ size ];  
    Random random = new Random();  
  
    /* 일반 배열의 정렬 소요시간 */    // numbers 배열에 랜덤한 값으로 초기화  
    IntStream.range( 0, size ).forEach( i -> numbers[ i ] = random.nextInt() );  
    long start = System.nanoTime();  
    // QuickSort를 사용하여  O(n log(n))의 복잡도( 최악의경우 O(n^2) )를 가지지만 싱글쓰레드라 시간이 좀 걸린다  
    Arrays.sort( numbers );  
    System.out.println( "serial sorting took " + ( System.nanoTime() - start ) );  
  
    /* parallelSort 정렬 소요시간 */    IntStream.range( 0, size ).forEach( i -> numbers[ i ] = random.nextInt() );  
    start = System.nanoTime();  
    Arrays.parallelSort( numbers );  
    System.out.println( "parallel sorting took " + ( System.nanoTime() - start ) );  
}


>>serial sorting took 1224000
>>parallel sorting took 745000
```
- 정렬하는 배열의 크기에 따라 시간은 달라질 수 있지만, parallelSort가 늘 더 빠르다



# 출처(참고문헌)
- 

# 연결문서
- [ForkJoinPool](ForkJoinPool.md)
