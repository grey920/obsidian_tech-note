# 날짜 : 2022-11-23 06:32

# 주제 : 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 객체를 가볍게 만들어 메모리 사용을 줄이는 패턴
- 자주 변하는 속성(또는 외적인 속성) 과 변하지 않는 속성(또는 내적인 속성) 을 분리하고 재사용하여 메모리 사용을 줄일 수 있다.
```

![](../img/Pasted%20image%2020221123063905.png)

```ad-note
title: Effective Java
- 9p. 같은 객체가 자주 요청되는 상황이라면 플라이웨이트 패턴을 사용할 수 있다.
```


## 플라이웨이트 패턴?
: 객체가 담고 있는 필드들이 많고, 그 중 빈번하게 사용되는 무거운 필드들이 있는 경우 => 
1) 자주 변경되는 속성과 변경되지 않는 것들을 분리하고
2) 그 중 잘 변경되지 않는 속성을 (플라이웨이트 팩토리에) 잘 모아두고
3) 플라이웨이트 팩토리에서 꺼내다 쓰는 패턴


## 예시
### 1. 객체
- 플라이웨이트 패턴 적용 전
	```java
	public class Character_old {  
	    private char value;  
	  
	    private String color;  
	  
	    private String fontFamily;  
	  
	    private int fontSize;  
	  
	    public Character_old ( char value, String color, String fontFamily, int fontSize ) {  
	        this.value = value;  
	        this.color = color;  
	        this.fontFamily = fontFamily;  
	        this.fontSize = fontSize;  
	    }  
	}
	```

- 플라이웨이트 패턴 적용을 위해 필드를 합친다.
	```java
	public class Character {  
	    private char value;  
	  
	    private String color;  
	  
	//    private String fontFamily;  
	//  
	//    private int fontSize;  
	  
	    // fontFamily와 fontSize는 잘 바뀌지 않기 때문에 하나로 합침.  
	    private Font font;  
	  
	    public Character ( char value, String color, Font font ) {  
	        this.value = value;  
	        this.color = color;  
	        this.font = font;  
	    }  
	}
	```
- 합친 Font 객체
	```java
	public class Character {  
	    private char value;  
	  
	    private String color;  
	  
	//    private String fontFamily;  
	//  
	//    private int fontSize;  
	  
	    // fontFamily와 fontSize는 잘 바뀌지 않기 때문에 하나로 합침.  
	    private Font font;  
	  
	    public Character ( char value, String color, Font font ) {  
	        this.value = value;  
	        this.color = color;  
	        this.font = font;  
	    }  
	}
	```


### 2. 플라이웨이트 팩토리
```java
/**  
 * 플라이웨이트 패턴 > 플라이웨이트 팩토리  
 */  
public class FontFactory {  
  
    private static Map<String, Font> cache = new HashMap<>();  
  
    /**  
     * 플라이웨이트 패턴을 사용한 로직 메서드에 static 키워드를 붙여 "정적 팩토리 메서드"를 함께 사용하도록 함  
     * 이러면 같은 font가 들어왔을 때 새로 생성하지 않고 같은 인스턴스를 리턴하므로 객체가 더 가벼워진턴  
     */  
    public static Font getFont( String font ){  
        if ( cache.containsKey( font ) ) {  
            return cache.get( font );  
        }  
        else {  
            String[] split = font.split( ":" );  
            Font newFont = new Font( split[ 0 ], Integer.parseInt( split[ 1 ] ) );  
            cache.put( font, newFont );  
            return newFont;  
        }  
    }
}
```
- 정적 팩토리 메서드도 함께 사용하기 위해 static을 붙였지만, 안붙여도 플라이웨이트 패턴이다.

### 3. 클라이언트
```java
public class Client {  
  
    public static void main ( String[] args ) {  
        // FontFactory fontFactory = new FontFactory();  
 
        Character c1 = new Character( 'h', "white", FontFactory.getFont( "nanum:12" ) );  
        Character c2 = new Character( 'e', "white", FontFactory.getFont( "nanum:12" ) );  
        Character c3 = new Character( 'l', "white", FontFactory.getFont( "nanum:12" ) );  
    }  
}
```
- `FontFactory.getFont` -> 플라이웨이트 팩토리 메서드를 static으로 만들어서 이 3개의 Character가 하나의 인스턴스를 공유하도록 만들어서 더 가볍게 했다. 



# 출처(참고문헌)
- 

# 연결문서
- 
