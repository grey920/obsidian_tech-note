# 날짜 : 2022-09-27 22:40

# 주제 : #java8 #DateTime 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- Java8에서 추가된 날짜/시간 관련 클래스. LocalDateTime과 다른점은 `타임존` 또는 `시차` 개념을 가지고 있다는 것이다.
	- LocalDateTime에 시차 개념이 추가된 것!
```


## ZoneId와 ZoneOffset
### ZoneId : 타임존
- UTC와의 시간 차이를 타임존 코드로 나타낸다.
	- ex) `Asia/Seoul`
- 많은 도시들이 Summer Time을 시행하기 때문에 고정된 시차를 사용하지 않는다. -> 따라서 이런 도시를 대상으로 ZonedDateTime 객체를 사용한다면 ZoneOffset보다는 계절에 따른 시차를 알아서 처리해주는 
 ZoneId를 사용하는 편이 유리하다. 

### ZoneOffset : 시차
- UTC 기준으로 고정된 시간 차이를 양수 또는 음수로 나타낸다.
	- ex) `+0900`



# 출처(참고문헌)
- [ZonedDateTime 사용법](https://www.daleseo.com/java8-zoned-date-time/)

# 연결문서
- [자바8의 Date와 Time API](자바8의%20Date와%20Time%20API.md)
