# 날짜 : 2022-09-26 07:46

# 주제 : #java #DateTime  
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 자바8부터는 `java.time패키지`의 새로운 날짜/시간 API를 사용하자! (java.util 패키지는 더이상X)
	- 기계용 시간 : Instant
	- 인류용 시간 : LocalDateTime, ZonedDateTime
```


## 자바 8에 새로운 날짜/시간 API가 생긴 이유?
- **thread safe** - 그전까지 사용하던 `java.util.Date` 클래스는 `mutable(불변이 아님)`하기 때문에 쓰레드 세이프하지 않다. setTime()메소드로 인스턴스를 직접 바꿔버릴 수 있기 때문에 위험하다. 
	```java
	Date date = new Date();  
	long time = date.getTime();  
	System.out.println( "date = " + date );  
	System.out.println( "time = " + time );  
	  
	Thread.sleep( 1000 * 3 );  
	Date after3Seconds = new Date();  
	System.out.println( "after3Seconds = " + after3Seconds );  
	after3Seconds.setTime( time ); //같은 인스턴스인데 안에 값을 바꿔버렸다.  
	System.out.println( "after3Seconds = " + after3Seconds );
	

	/////////////////
	date = Mon Sep 26 08:03:19 KST 2022
	time = 1664146999742
	after3Seconds = Mon Sep 26 08:03:22 KST 2022
	after3Seconds = Mon Sep 26 08:03:19 KST 2022
	
	```
- 클래스 이름이 명확하지 않다. 
	- Date date = new Date(); 는 date.getTime();으로 시간까지 다룰 수 있으나 이름은 Date이다. 날짜에서 시간을 가져오는게 이상하다. 
	- getTime()으로 받는 시간은 [epoch Time](epoch%20Time.md)이다. (기계용 시간)
- 버그 발생할 여지가 많다. 
	- month에 보면 인텔리제이가 상수를 쓰라고 경고를 준다. ( `월이 0부터 시작`하기 때문에 1월 18일을 의도했을때 그 의도와 다르게 이 값은 2월이 된다. )
		![](../img/Pasted%20image%2020220926080813.png)
	- 타입 세이프티하지 않다. int 타입으로만 받고 있기 때문에 사실상 -10이 들어와도 되는 것이다. => Month타입의 enum 타입을 써야 한다.
		![](../img/Pasted%20image%2020220926081243.png)



## 바뀐 자바8의 Date-Time API
### 1. 기계용 시간과 인류용 시간
- **기계용 시간**: 에폭([epoch Time](epoch%20Time.md) - 1970년 1월 1일 0시 0분 0초)부터 현재까지의 타임스탬프를 표현
	- `Instant` 사용 : Instant.now()는 현재 UTC(GMT) 를 리턴한다.
		- UTC(Universal Time Coordinated) == GMT(Greenwich Mean Time)
	```java
	// 기계적 시간 API
	// of() => 특정 에폭 시간을 기준으로 만든다  
	Instant instant = Instant.now(); 
	System.out.println( "instant = " + instant ); // 기준시 UTC ( == 그리니치 GMT )
	System.out.println( instant.atZone( ZoneId.of( "UTC" ) ) ); // 위와 같다


	>> instant = 2022-09-27T12:26:27.465714Z
	>> 2022-09-27T12:26:27.465714Z[UTC]
	```

	```java
	// UTC를 내 로컬 기준으로 보기  
	ZoneId zone = ZoneId.systemDefault();  
	System.out.println( "zone = " + zone ); 
	// 어느 zone을 기준으로 현재 시간을 볼지를 설정한다  
	ZonedDateTime zonedDateTime = instant.atZone( zone );
	System.out.println( "zonedDateTime = " + zonedDateTime );
	
	>> zone = Asia/Seoul
	>> zonedDateTime = 2022-09-27T21:26:27.465714+09:00[Asia/Seoul]		
	```

- **인류용 시간**: 흔히 사용하는 연, 월, 일, 시, 분, 초 등을 표현. 해당 소스가 돌아가는 서버의 시스템 zone 시간대를 사용한다
	- 특정 날짜 : `LocalDate`
		```java
		LocalDate now = LocalDate.now(); 
		System.out.println( "now = " + now );
		
		>> now = 2022-09-27
		```
	- 시간 : `LocalTime`
	- 일시 : `LocalDateTime`
		- LocalDateTime.now() : 현재 시스템 Zone에 해당하는 (로컬) 일시를 리턴
		- LocalDateTime.of(int, Month, int, int, int, int) : 로컬의 특정 일시를 리턴
			```java
			LocalDateTime birthDay = LocalDateTime.of( 1992, 1, 18, 0, 0, 0 );  
			System.out.println( "birthDay = " + birthDay );
			
			>> birthDay = 1992-01-18T00:00
			```
		- [ZonedDateTime](ZonedDateTime.md).of(int, Month, int, int, int, int, ZoneId) : 특정 Zone의 특정 일시를 리턴
			```java
			// 특정 zone의 시간을 보고싶다면,,  
			ZonedDateTime nowInKorea = ZonedDateTime.now( ZoneId.of( "Asia/Seoul" ) );  
			System.out.println( "nowInKorea = " + nowInKorea );  
			ZonedDateTime birthDayZonedDateTime = ZonedDateTime.of( birthDay, ZoneId.of( "Asia/Seoul" ) );  
			System.out.println( "birthDayZonedDateTime = " + birthDayZonedDateTime );
			
			>> nowInKorea = 2022-09-27T21:43:31.284257+09:00[Asia/Seoul]
			>> birthDayZonedDateTime = 1992-01-18T00:00+09:00[Asia/Seoul]
			>> zonedDateTime1 = 2022-09-27T21:43:31.286611+09:00[Asia/Seoul]
			```

- Instant <-> ZonedDateTime <-> LocalDateTime 변환 가능
	```java
	// Instant -> ZonedDateTime
	Instant nowInstant = Instant.now();  
	ZonedDateTime zonedDateTime1 = nowInstant.atZone( ZoneId.of( "Asia/Seoul" ) );  
	System.out.println( "zonedDateTime1 = " + zonedDateTime1 );
	// ZonedDateTime -> LocalDateTime
	LocalDateTime localDateTime1 = zonedDateTime1.toLocalDateTime();  
	System.out.println( "localDateTime1 = " + localDateTime1 );
	
	>> zonedDateTime1 = 2022-09-27T22:36:35.927109+09:00[Asia/Seoul]
	>> localDateTime1 = 2022-09-27T22:36:35.927109
	```
	ZonedDateTime에서 타임존 코드가 빠진게 LocalDateTime


### 2. 기간 표현법 두 가지
- 기간 표현
	- 시간 기반(기계용) : **Duration**
		- Duration.between()
		```java
		Instant nowInstantForBetween = Instant.now();  
		Instant plus = nowInstantForBetween.plus( 10, ChronoUnit.SECONDS );  
		Duration between = Duration.between( nowInstantForBetween, plus );  
		System.out.println( "between = " + between.getSeconds() );
		
		>> between = 10
		```
	- 날짜 기반(사람용) : **Period**
		- Period.between()
		```java
		// 오늘부터 다음해 생일까지 차이
		LocalDate today = LocalDate.now();  
		LocalDate nextYearBirthday = LocalDate.of( 2023, Month.JANUARY, 18 );  
		  
		Period period = Period.between( today, nextYearBirthday );  
		System.out.println( period.getDays() );  
		  
		Period until = today.until( nextYearBirthday );  
		System.out.println( until.get( ChronoUnit.DAYS ));
		// Period는 기간을 연, 월, 일로 표현하기 때문에 30일이 넘어간 정보는 '월'에 담긴다
		
		>> 22
		>> 22
		```
		- 전체 일수를 계산하고 싶다면 ChronoUnit이 제공하는 between()을 사용한다. `Period와 Duration`의 between()은 년, 달, 일, 초의 `단순 차이`를 리턴하고, `ChronoUnit` 열거 타입의 between()은 `전체 시간을 기준`으로 차이를 리턴한다. 
		```java
		System.out.println( ChronoUnit.DAYS.between( today, nextYearBirthday ) );
		
		>> 113
		```

### 3. 포맷팅 ( Date <-> String )
- DateTimeFormatter를 사용해서 일시를 -> 문자열로 formatting 할 수 있다.
- Date -> text (formatting)
	```java
	LocalDateTime localDateTimeNow = LocalDateTime.now();
	// 원하는 모양으로 출력하고 싶을 때 DateTimeFormatter (문자열로 변환)  
	DateTimeFormatter MMddyyyy = DateTimeFormatter.ofPattern( "MM/dd/yyyy" );  
	System.out.println( "Formatted localDateTimeNow = " + localDateTimeNow.format( MMddyyyy ) );
	
	>> localDateTimeNow = 2022-09-27T22:36:35.958301
	>> Formatted localDateTimeNow = 09/27/2022
	```
	- 미리 정의되어있는 타입이라면 문서 참고 https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#predefined  
- text -> Date (parsing)
	```java
	LocalDate parse = LocalDate.parse( "09/27/2022" , MMddyyyy );  
	System.out.println( "parse = " + parse );
	
	>> parse = 2022-09-27
	```

![](../img/Pasted%20image%2020220927212017.png)
[datetime/iso/overview](https://docs.oracle.com/javase/tutorial/datetime/iso/overview.html)


### 4. 레거시 API 지원
- Instant로만 변환할 수 있다면 최신 API로 다 바꿀 수 있다.
- Date (old) <-> Instant (new)
	```java
	Date date = new Date();  
	Instant toInstant = date.toInstant(); // date -> instant  
	Date toDate = Date.from( toInstant ); // instant -> date
	
	System.out.println( "toInstant = " + toInstant );  
	System.out.println( "toDate = " + toDate ); 
	
	>> toInstant = 2022-09-27T13:36:35.961Z
	>> toDate = Tue Sep 27 22:36:35 KST 2022
	```
- GregorianCalendar(old) <-> ZonedDateTime, LocalDateTime(new)
	```java
	GregorianCalendar gregorianCalendar = new GregorianCalendar(); 
	// GregorianCalendar -> ZonedDateTime   
	ZonedDateTime dateTime = gregorianCalendar.toInstant().atZone( ZoneId.systemDefault() ); 
	System.out.println( "dateTime = " + dateTime );  
 
	 // ZonedDateTime -> GregorianCalendar  
	GregorianCalendar from = GregorianCalendar.from( dateTime );
	System.out.println( "from = " + from.getTime() );
	
	>> dateTime = 2022-09-27T22:36:36.023+09:00[Asia/Seoul]
	>> from = Tue Sep 27 22:36:36 KST 2022
	```
- TimeZone(oid) <-> ZoneId(new)
	```java
	// TimeZone(old) -> ZoneId(new)  
	ZoneId zoneId = TimeZone.getTimeZone( "PST" ).toZoneId(); 
	System.out.println( "zoneId = " + zoneId );  
	
	// ZoneId -> TimeZone  
	TimeZone timeZone = TimeZone.getTimeZone( zoneId ); 
	System.out.println( "timeZone = " + timeZone.getID() );
	
	>> zoneId = America/Los_Angeles
	>> timeZone = America/Los_Angeles
	```


### 5. 연산
- plus()
```java
LocalDateTime localDateTime = LocalDateTime.now();   
LocalDateTime localDatePlus = localDateTime.plus( 10, ChronoUnit.DAYS );  
System.out.println( "localDatePlus = " + localDatePlus );

>> localDatePlus = 2022-10-07T22:36:36.030156
```
- `[Tip!]` plus()의 두번째 인자로 TemporalUnit을 넣게 되어있지만, TemporalUnit을 쓰면 더 쓸 수 있는게 없기 때문에 대신 **ChronoUnit**을 사용하면 편하다  
- `[Warning!]` **localDateTime**은 Immutable하기 때문에 반드시 **인스턴스를 새로 만든 것으로 사용**해야 한다!!! 옛날 API처럼 localDateTime.plus()만 쓰고 끝내면 아무 동작도 하지 않는다.  


# 출처(참고문헌)
- [Class DateTimeFormatter](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#predefined)

# 연결문서
-  [ALL ABOUT JAVA.UTIL.DATE](https://codeblog.jonskeet.uk/2017/04/23/all-about-java-util-date/)
