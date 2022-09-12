## 날짜 : 2022-09-09 21:10

## 주제 : #java8 #함수형프로그래밍 
----
## 메모
```ad-note
title: tl;dr
- 익명 클래스의 가독성을 보완하기 위해 만들어졌다. 
```


### 람다 표현식
- 함수형 인터페이스의 인스턴스를 만드는 방법으로 쓰일 수 있다. ( 구현 방법으로는 익명 클래스 / 람다식이 있다)
- 코드를 줄일 수 있다.
- 메소드 매개변수, 리턴 타입, 변수로 만들어 사용할 수도 있다. 


### 모양
- ( 인자 리스트 ) -> { 바디 }
- 인자의 타입은 생략 가능하다. 컴파일러가 추론하지만 명시할 수도 있다. 
```java
public class Foo {  
  
    public static void main ( String[] args ) {  
		// 바디가 한 줄인 경우 {}와 return은 생략 가능. 
        BinaryOperator<Integer> multiply = ( a, b ) -> a * b;  
        Supplier<Integer> return10 = () -> {  
            return 10;  
        };  
  
    }  
}
```



### [[변수 캡처 (variable capture)]] 
```java
public class Foo {  
  
    public static void main ( String[] args ) {  
  
        Foo foo = new Foo();  
        foo.run();  
  
    }  
  
    private void run () {  
  
        int baseNum = 10;  
  
        IntConsumer printInt = ( i ) -> {  
		// 여기서 참조하는 로컬 변수인 baseNum은 캡쳐가 된다. (변수 캡쳐 variable capture)            
			System.out.println( i + baseNum );  
        };  
  
        printInt.accept( 5 );  
    }  
}
```

- [[람다 캡처링]]


```ad-note
즉, 람다식은 외부 블록에 있는 변수에 접근할 수 있으며,
외부에 있는 변수가 로컬 변수일 경우 final 혹은 effective final인 경우에만 참조 가능하다.
```



### 익명 내부 클래스와 비교
| 익명 내부 클래스                                                            | 람다 표현식                          |
| --------------------------------------------------------------------------- | ------------------------------------ |
| 이름이 없는 클래스                                                          | 이름이 없는 메서드                   |
| 추상 및 구체 클래스를 확장할 수 있다.                                       | 추상 및 구체 클래스를 확장할 수 없다 |
| 익명 내부 클래스 내부에서 인스턴스 변수를 선언할 수 있다                    | 인스턴스 변수를 선언할 수 없다.(은닉 변수 [shadowing 쉐도잉](shadowing%20쉐도잉.md)를 허용X)       |
| 익명 내부 클래스를 인스턴스화 할 수 있다                                    | 람다 표현식을 인스턴스화 할 수 없다  |
| 익명 내부 클래스 내부의 `this` 키워드는 현재 익명 내부 클래스 객체(자기자신)를 참조한다 | 람다 표현식 내부의 `this` 키워드는 현재 외부 클래스 객체(선언된 클래스)를 나타낸다                                     |


## 출처(참고문헌)
- [Java 익명 내부 클래스와 람다식의 차이점](https://developer-talk.tistory.com/499)
- [lambda 와 effectively final](https://vagabond95.me/posts/lambda-with-final/)

## 연결문서
- [[익명 내부 클래스]]
