# 날짜 : 2022-09-16 07:55

# 주제 : 
----
# 메모
```ad-note
title: tl;dr
- 해당 타입 관련 헬퍼 또는 유틸리티 메소드를 제공할 때 인터페이스에 static 메소드를 제공할 수 있다.
```


## 왜 사용하는가
- 해당 인터페이스를 구현하는 모든 인스턴스, 또는 해당 타입에 관련된 유틸리티나 헬퍼 메서드를 제공하고 싶은 경우 사용한다.


## 방법

### 1) 인터페이스 내에서 static 메서드 구현
```java
public interface Foo {  
  
    // 필수로 구현해야 함  
    void printName();  
  
    // 인스턴스들이 공통적으로 제공해줬으면 하는 기능이라면 -> Default 메서드 사용  
  
    /**  
     * @implSpec getName()으로 리턴받은 문자열을 대문자로 변환하여 출력한다.  
     */    default void printNameUpperCase () {  
        System.out.println( getName().toUpperCase() );  
    }  
  
  
    /**  
     * static 메서드 : 해당 타입 관련 헬퍼 또는 유틸리티 메서드를 제공할 떄 사용  
     */  
    static void printAnything() {  
        System.out.println("Foo");  
    }  
  
    String getName();  
}
```


### 2) 사용하는 쪽에서 `인터페이스명.스태틱메소드()` 로 사용
```java
public class App {  
  
    public static void main ( String[] args ) {  
    
        // static메서드 사용  
        Foo.printAnything();  
  
    }  
}
```




# 출처(참고문헌)
- 

# 연결문서
- 
