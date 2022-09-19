# 날짜 : 2022-09-19 21:31

# 주제 : #java8 #스트림API 
----
# 메모
```toc
```

```ad-note
title: tl;dr
- 스트림 API에서 다른 Stream으로 변환하는데 사용되는 파이프라인의 중간 작업.
- [종료 오퍼레이션 (terminal operation)](종료%20오퍼레이션%20(terminal%20operation).md)이 실행되기 전까지는 호출되지 않기 때문에 결과를 생성하지 않는다. 
```

## 중개 오퍼레이션이란
- **Stream을 리턴한다.**
- Stateless / Stateful 오퍼레이션으로 더 상세하게 구분할 수도 있다.
	- 대부분은 Stateless지만, distinct나 sorted처럼 이전 이전 소스 데이터를 참조해야 하는 오퍼레이션은 Stateful 오퍼레이션이다.
- filter, map, limit, skip, sorted, ...


### 예제 코드
```java
public class App {  
  
    public static void main ( String[] args ) {  
  
        List<String> names = new ArrayList<>();  
        names.add( "정겨운" );  
        names.add( "정다와" );  
        names.add( "정중기" );  
        names.add( "이미자" );  
  
        /**  
         * 여기서 중개 오퍼레이션인 map() 내의 출력문은 출력되지 않는다.  
         * 중개 오퍼레이션은 종료 오퍼레이 오기 전까지는 실행하지 않기 때문이다.  
         */        Stream<String> stringStream = names.stream()  
                .map( s -> {  
                    System.out.println(s);  
                    return s.toUpperCase();  
                } );  
  
        System.out.println("===========================");  
  
        names.forEach( System.out::println );  
  
    }  
}

```

실행해보면 실제로 map() 내의 출력문은 찍히지 않는다.
```
===========================
정겨운
정다와
정중기
이미자
```





# 출처(참고문헌)
- 

# 연결문서
- [스트림 API](스트림%20API.md)
- [종료 오퍼레이션 (terminal operation)](종료%20오퍼레이션%20(terminal%20operation).md)