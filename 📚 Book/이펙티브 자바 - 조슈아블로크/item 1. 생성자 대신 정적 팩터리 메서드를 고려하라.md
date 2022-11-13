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

이럴때 정적 팩터리 메서드를 사용하면 이름으로 표현할 수 있다. 
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
- 인스턴스 통제 클래스 : 반복되는 요청에 같은 객체를 반환 =>  `언제 어느 인스턴스를 살아 있게 할지를 철저히 통제`할 수 있다. 
	- 왜 통제해야 할까?
	- 인스턴스를 통제하면 1) 클래스를 싱글턴 아이템으로 만들 수도, 2) 인스턴스화 불가 아이템으로 만들수도 있다. 3) 불변  값 클래스에서 동치인 인스턴스가 단 하나뿐임을 보장할 수도 있다. 
	- 인스턴스 통제는 플라이웨이트 패턴의 근간이 되며, 열거 타입은 인스턴스가 하나만 만들어짐을 보장한다.
```ad-question
title: 인스턴스 통제
열거 타임은 인스턴스가 하나만 만들어짐을 보장한다고?? 왜지?? 
```
### 3) 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
- 반환할 객체의 클래스를 자유롭게 선택할 수 있다. -> 엄청난 유연성!
- 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있다 => API를 작게 유지할 수 있다
	- => API가 작아지면 프로그래머가 API를 사용하기 위해 익혀야 하는 개념의 수와 난이도도 낮아진다.
- 정적 팩터리 메서드를 사용하는 클라이언트는 얻은 객체를 구현 클래스가 아닌 인터페이스만으로 다루게 된다. ( 좋은 습관 )
### 4) 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
- 반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다. 
- 클라이언트는 팩터리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 수도 없고 알 필요도 없다. 반환 타입의 하위 클래스이기만 하면 되는 것이다. 
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

## 정적 팩터리 메서드의 단점
### 1) 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다. 
- 예를 들어 컬렉션 프레임워크의 유틸리티 구현 클래스들은 상속할 수 없다. 
- [상속보다 컴포지션을 사용](item%2018.%20상속보다는%20컴포지션을%20사용하라.md)하도록 유도하고 [불변 타입](item%2017.%20변경%20가능성을%20최소화하라.md)으로 만들려면 오히려 장점으로 받아들여질수도 있다.

### 2) 정적 팩터리 메서드는 프로그래머가 찾기 어렵다. 
- 생성자처럼 API 설명에 명확히 드러나지 않으니 사용자는 정적 팩터리 메서드 방식 클래스를 인스턴스화할 방법을 `알아내야` 한다.
- API 문서를 잘 써놓고 메서드 이름도 널리 알려진 규약에 따라 짓는 식으로 문제를 완화해줘야 한다. 
```ad-attention
title: 정적 팩터리 메서드에 흔히 사용하는 명명 방식들
- from 
- of
- valueOf
- instance 혹은 getInstance
- create 혹은 newInstance
- getType
- newType
- type
```

## Keyword
- 플라이웨이트 패턴 (Flyweight pattern) 9p
- 인스턴스 통제 클래스(instance-controlled) 9p
- 서비스 제공자 프레임워크(service provider framework) 11p


# 출처(참고문헌)
- 

# 연결문서
- [item 18. 상속보다는 컴포지션을 사용하라](item%2018.%20상속보다는%20컴포지션을%20사용하라.md)
- [item 17. 변경 가능성을 최소화하라](item%2017.%20변경%20가능성을%20최소화하라.md)