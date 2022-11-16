# 날짜 : 2022-11-13 23:07

# 주제 : #객체생성과_파괴
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 클래스의 인스턴스를 얻는 방식에는 public 생성자와 정적 팩터리 메서드가 있다. 
- 정적 팩터리를 사용하는게 유리한 경우가 더 많으므로 무지성으로 public 생성자를 사용하던 습관을 고치자
```

## 클래스의 인스턴스를 얻는 방법
1. 전통적인 방식: public 생성자
2. 정적 팩터리 메서드 (static factory method) 사용
클래스는 클라이언트에 public 생성자 대신 (혹은  생성자와 함께) 정적 팩터리 메서드를 제공할 수 있다. 

## 정적 팩터리 메서드의 5가지 장점
### 1) 이름을 가질 수 있다.
- 생성자는 클래스 이름과 동일하게만 가야한다는 단점이 있다. 
- 만약 생성자의 시그니처가 중복되는 경우에 정적 팩터리 메서드를 사용하는게 더 좋다 .
```java
public class Order {  
  
    private boolean prime;  
  
    private boolean urgent;  
  
    private Product product;  
  
    private OrderStatus orderStatus;  
  
    public Order ( boolean prime, Product product ) {  
        this.prime = prime;  
        this.product = product;  
    }
}

```
 prime을 받는 기본 생성자가 존재하는 상황에서 urgent를 받고 싶을 때,
```java
public Order ( boolean prime, Product product ) {  
    this.prime = prime;  
    this.product = product;  
}  
  
public Order ( boolean urgent, Product product ) {  
    this.urgent = urgent;  
    this.product = product;  
}
```
이러면 시그니처가 같기 때문에 컴파일 에러가 발생한다. 
파라미터의 순서를 바꿈으로써 컴파일에러를 피해갈 순 있지만 생성자는 이름이 똑같기 때문에 좋지 않다.
```java
public Order ( boolean prime, Product product ) {  
    this.prime = prime;  
    this.product = product;  
}  
  
public Order ( Product product, boolean urgent ) {  
    this.urgent = urgent;  
    this.product = product;  
}
```

이럴때 정적 팩터리 메서드를 사용하면 만드는 객체의 특징을 이름으로 잘 표현할 수 있다. 
```java
public class Order {  
  
    private boolean prime;  
  
    private boolean urgent;  
  
    private Product product;  
  
    private OrderStatus orderStatus;  
  
    public static Order primeOrder(Product product) {  
        Order order = new Order();  
        order.prime = true;  
        order.product = product;  
  
        return order;  
    }  
  
    public static Order urgentOrder(Product product) {  
        Order order = new Order();  
        order.urgent = true;  
        order.product = product;  
        return order;  
    }
```

### 2) 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다. 
- `생성자`를  통해 만들면 인스턴스마다 새로운 객체가 만들어진다. 이러한 방법은 인스턴스의 생성 **통제가 불가능**하다.
	- 만약 Settings 객체가 바뀌면 안되고 *딱 하나만 만들어 사용*해야 한다면?? => **정적 팩토리**를 사용!
	```java
	public static void main ( String[] args ) {  
	    System.out.println( new Settings() );  
	    System.out.println( new Settings() );  
	    System.out.println( new Settings() );  
	}
	
	>>>
	me.whiteship.chapter01.item01.Settings@3796751b
	me.whiteship.chapter01.item01.Settings@67b64c45
	me.whiteship.chapter01.item01.Settings@4411d970
	```
- `인스턴스 통제 클래스` : 반복되는 요청에 같은 객체를 반환 =>  `언제 어느 인스턴스를 살아 있게 할지를 철저히 통제`할 수 있다. 이 경우, **클라이언트는 인스턴스를 가져올수는 있지만 생성할수는 없다!**
	- 왜 통제해야 할까?
	- 인스턴스를 통제하면 1) 클래스를 싱글턴 아이템으로 만들 수도, 2) 인스턴스화 불가 아이템으로 만들수도 있다. 3) 불변  값 클래스에서 동치인 인스턴스가 단 하나뿐임을 보장할 수도 있다. 
	- 인스턴스 통제는 플라이웨이트 패턴의 근간이 되며, 열거 타입은 인스턴스가 하나만 만들어짐을 보장한다.
```ad-question
title: 인스턴스 통제
열거 타입은 인스턴스가 하나만 만들어짐을 보장한다고?? 왜지?? 
- enum 클래스는 고정된 상수들의 집합으로, 런타임이 아닌 `컴파일 시점`에 모든 값을 알고 있어야 한다.
- 즉, 다른 패키지나 클래스에서 enum 타입에 접근해서 동적으로 어떤 값을 정해줄 수 없으며 따라서 타입 안정성이 보장된다. ( 해당 enum 클래스 내에서도 new 키워드, newInstance(), clone() 등의 메소드 사용으로 인스턴스 생성 X )
- enum 클래스는 **생성자의 접근 제어자를 private으로 설정**해야 하기 때문에 결국 인스턴스 생성을 제어라고 싱글톤을 일반화 한다고 볼 수 있다.
```

- 어떻게 만들까
	- 1. private 생성자 제공 : 외부에서 아무도 변경하지 못하도록 생성자를 private으로 만든다.
	- 2. 객체를 미리 만들어 놓기 
	- 3. 정적 팩토리 제공
- 전제 코드
	- 정적 팩토리로 생성된 객체들은 모두 같은 해시값을 가지고 있다.
	```java
	  
	/**  
	 * 이 클래스의 인스턴스는 #getInstance()를 통해 사용한다.  
	 * @see #getInstance()  
	 */
	 public class Settings {  
	  
	    private boolean useAutoSteering;  
	  
	    private boolean useABS;  
	  
	    private Difficulty difficulty;  
	  
	    // 1) private 생성자를 만들어 외부에서 변경할 수 없도록 한다.  
	    private Settings() {}  
	  
	    // 2) 미리 만들어놓는다.  
	    private static final Settings SETTINGS = new Settings();  
	  
	    // 3) 정적 팩토리를 제공한다  
	    public static Settings getInstance() {  
	        return SETTINGS;  
	    }  
	  
	    public static void main ( String[] args ) {  
	        System.out.println( new Settings() );  
	        System.out.println( new Settings() );  
	        System.out.println( new Settings() );  
	  
	        System.out.println(">>>>>>>>>>>>>>>>>>>>");  
	  
	        System.out.println( Settings.getInstance() );  
	        System.out.println( Settings.getInstance() );  
	        System.out.println( Settings.getInstance() );  
	    }  
	  
	}
	>> 
	me.whiteship.chapter01.item01.Settings@3796751b
	me.whiteship.chapter01.item01.Settings@67b64c45
	me.whiteship.chapter01.item01.Settings@4411d970
	>>>>>>>>>>>>>>>>>>>>
	me.whiteship.chapter01.item01.Settings@6442b0a6
	me.whiteship.chapter01.item01.Settings@6442b0a6
	me.whiteship.chapter01.item01.Settings@6442b0a6
	```
- 생성하는 로직을 컨트롤하는 것은 정적 팩토리 내에서 가능하다. 
	- 예를 들어, Boolean의 valueOf()의 경우 매개변수에 따라 미리 만들어놓은 상수를 각각 리턴한다. 
		![](../../img/Pasted%20image%2020221115071450.png)
		![](../../img/Pasted%20image%2020221115071529.png)
	- 즉, 정적 팩토리 메서드를 사용하면 매개변수에 따라 다른 인스턴스를 리턴하는 것이 가능해진다

### 3) 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
- 반환할 객체의 클래스를 자유롭게 선택할 수 있다. -> 엄청난 **유연성**!
	- 생성자로  만드는 경우, 무조건 본인 타입으로 리턴할 수 밖에 없다.
	- => API가 작아지면 프로그래머가 API를 사용하기 위해 익혀야 하는 개념의 수와 난이도도 낮아진다.
- 정적 팩터리 메서드를 사용하는 클라이언트는 얻은 객체를 구현 클래스가 아닌 인터페이스만으로 다루게 된다. ( 좋은 습관 )

### 4) 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
- 반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다. 
- 클라이언트는 팩터리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 수도 없고 알 필요도 없다. 반환 타입의 하위 클래스이기만 하면 되는 것이다. 
```java
public class HelloServiceFactory {  
	public static HelloService of ( String lang ){  
		if ( lang.equals( "ko" ) ) {  
			return new KoreanHelloService();  
		}  
		else {  
			return new EnglishHelloService();  
		}  
	}
}
```
### 5) 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다. 
- 이런 유연함은 서비스 제공자 프레임워크를 만드는 근간이 된다. 
- 대표적인 서비스 제공자 프레임워크 : JDBC
- 서비스 제공자 프레임워크의 **3가지 핵심 컴포넌트**
	- `서비스 인터페이스` : 구현체의 동작을 정의
		- JDBC에서는 Connection
	- `제공자 등록 API` : 제공자가 구현체를 등록할 때 사용
		- JDBC의 DriverManager.registerDriver
	- **서비스 접근 API** : 클라이언트가 서비스의 인스턴스를 얻을 때 사용
		- JDBC에서 DriverManager.getConnection
- 클라이언트는 서비스 접근 API를 사용할 때 원하는 구현체의 조건을 명시할 수 있다. 
- 이 `서비스 접근 API`가 서비스 제공자 프레임워크의 근간이라고 한 ``'유연한 정적 팩터리'의 실체`다.
- +) 3개의 핵심 컴포넌트와 더불어 종종 `서비스 제공자 인터페이스`라는 네 번째 컴포넌트가 쓰이기도 한다. 
	- 이 컴포넌트는 서비스 인터페이스의 인스턴스를 생성하는 팩터리 객체를 설명해준다. 
	- JDBC에서 Driver

- **예시**
- [서비스 로더 (Service Loader)](../../0.Inbox(fleeting%20note.literature%20note)/서비스%20로더%20(Service%20Loader).md): 자바가 기본으로 제공하는 정적 팩터리 메서드
	![](../../img/Pasted%20image%2020221116074236.png)
```java
public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {  
  
    // ServiceLoader : 자바가 기본으로 제공하는 정적 팩토리 메서드  
    // ServiceLoader.load( HelloService.class ) : HelloService 타입의 임의의 서비스를 로딩한다.  
    // 아래 코드가 실행되는 순간, 현재 참조할 수 있는 클래스패쓰 내에 있는 모든 HelloService의 구현체들을 가져온다 (iterable)    
    ServiceLoader<HelloService> loader = ServiceLoader.load( HelloService.class );  
  
    // 구현체 중 가장 첫번째 것을 가져온다. 없을수도 있기 때문에 Optional을 사용함  
    Optional<HelloService> helloServiceOptional = loader.findFirst();  
    // 있으면 호출해라  
    helloServiceOptional.ifPresent( helloService -> {  
        System.out.println( helloService.hello() );  
    } );
}

>>> Ni Hao
```

helloServiceOptional에 존재하지 않는 ChineHelloService 클래스가 들어있다.
현재 **정적 팩토리가 있는 상태에서 구현체가 없는 상황**인 것이다. `인터페이스`만 있으면 되는 것이다.
![](../../img/Pasted%20image%2020221116075055.png)

- 존재하지 않는 ChineHelloService 클래스는 어디에서 어떻게 왔을까?
서비스 제공자 프레임워크의 자바 기본 구현체인 `ServiceLoader`를 사용한 것.
1) ChineHelloService는 다른 프로젝트에 구현해 놓았다.
![](../../img/Pasted%20image%2020221116080052.png)

2) 그리고 이 ChineHelloService를 등록해 놓았다. ( resources/META-INF/services/FQFN클래스명)
	- 파일 안에 `풀 패키지 경로 + 클래스명` 까지 써주면 jar 로 패키징할 때 이 정보가 jar 안에 들어가고, 그럼 다른 프로젝트에서 참조해서 사용할 수 있게된다.
![](../../img/Pasted%20image%2020221116075954.png)

3) 다시 ServiceLoader로 사용하는 쪽 pom.xml에 ChineHelloService를 패키징한 jar를 넣어 참조한다.
![](../../img/Pasted%20image%2020221116080733.png)

- **어차피 디펜던시로 ChineHelloService를 참조한다면 new 생성자로 바로 쓰면되지, 왜 저런식으로 사용하는가??**
	```java
	ChineseHelloService helloService = new ChineseHelloService();  
	System.out.println( helloService.hello() );
	```
	- 이 코드를 써도 된다. 하지만 이 코드는 `ChineseHelloService`에 **의존적**이다.
		- 이렇게 쓰면 ChineseHelloService를 import 해야한다. 이는 ChineseHelloService를 사용하겠다고 명확하게 표현한 것. (의존성 생김)
	- 앞에서 **ServiceLoader**를 사용한 코드는 `ChineseHelloService`에 **의존적이지 않다**.
		- `어떤 ServiceLoader는 어떤 구현체가 올지 코드상에서 알 수가 없다.`
		- -> 어떤 구현체가 올 지 모르지만 이 구현체가 따르는 인터페이스 기반으로만 코딩하면 되는 경우 이런 식으로 쓴다. 
		- 예) JDBC 드라이버. 어떤  DB를 사용할 지 코드는 모르는 상황.



## 정적 팩터리 메서드의 단점
### 1) 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다. 
- 예를 들어 컬렉션 프레임워크의 유틸리티 구현 클래스들은 상속할 수 없다. 
- [상속보다 컴포지션을 사용](item%2018.%20상속보다는%20컴포지션을%20사용하라.md)하도록 유도하고 [불변 타입](item%2017.%20변경%20가능성을%20최소화하라.md)으로 만들려면 오히려 장점으로 받아들여질수도 있다.
- 예를 들어 정적 팩터리 메서드를 사용하는 Settings 라는 클래스를 상속해서 AdvancedSettings 클래스를 만들고 싶어도 Settings 클래스의 생성자는 private이라 상속을 할 수 없다.
- 대신 우회하는 방법으로 AdvancedSettings에 Settings를 delegation으로 가지고 있고, 이 안에서 settings를 변경하게 하면 굳이 상속을 안해도 그 기능을 사용할 수 있다. 
	```java
	public class AdvancedSettings {  
  
	    Settings settings;  
  
	}
	```


```ad-note
- static 팩터리 메서드를 제공하면서도 public한 생성자를 제공할 수도 있다!
- 그 예로 List는 new로도 만들 수 있지만, List.of()를 사용해서도 만들 수 있다.
```

### 2) 정적 팩터리 메서드는 프로그래머가 찾기 어렵다. (문서화 관련)
- 생성자처럼 API 설명에 명확히 드러나지 않으니 사용자는 정적 팩터리 메서드 방식 클래스를 인스턴스화할 방법을 `알아내야` 한다.
- **API 문서를 잘 써놓고** 메서드 이름도 널리 알려진 규약에 따라 짓는 식으로 문제를 완화해줘야 한다. ( **네이밍 패턴** )
```ad-attention
title: 정적 팩터리 메서드에 흔히 사용하는 명명 방식들
- from 
- of
	- 매개변수를 받아서 생성하는 경우
- valueOf
- instance 혹은 getInstance
	-  미리 만들어져있는 인스턴스를 가져오는 경우
- create 혹은 newInstance
	- 매번 팩토리 안에서 새로 만드는 경우
- getType
- newType
- type
```

```ad-info
title: 문서화(주석)관련
- @see 
- `#` : 자신이 가지고 있는 메서드를 참조할 때는 `#getInstance()` 이런 식으로 참조.
	- 밖에 있는 클래스를 참조하려면 `Settings#getInstance()` 이런 식으로 작성
```


## Keyword
- 플라이웨이트 패턴 (Flyweight pattern) 9p
	- 우리가 자주 사용하는 답들을 미리 캐싱해서 넣어놓고 꺼내다 쓰는 형식
	- 미리 자주 사용하는 객체들을 만들어놓고 필요로 하는 인스턴스를 꺼내다 쓰기 때문에 언급됨
- 인스턴스 통제 클래스(instance-controlled) 9p
- 서비스 제공자 프레임워크(service provider framework) 11p
- [서비스 로더 (Service Loader)](../../0.Inbox(fleeting%20note.literature%20note)/서비스%20로더%20(Service%20Loader).md)
- 

# 출처(참고문헌)
- 

# 연결문서
- [item 18. 상속보다는 컴포지션을 사용하라](item%2018.%20상속보다는%20컴포지션을%20사용하라.md)
- [item 17. 변경 가능성을 최소화하라](item%2017.%20변경%20가능성을%20최소화하라.md)