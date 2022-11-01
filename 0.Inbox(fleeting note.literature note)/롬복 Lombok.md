# 날짜 : 2022-11-02 07:06

# 주제 : 
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 표준적으로 작성해야 하는 코드를 개발자 대신 생성해주는 툴
- 예. Getter, Setter, Constructor, ...
```

## 인텔리제이에서 롬복 사용하기
1. pom.xml에 디펜던시 추가
2. [인텔리제이] plugins에 Lombok 설치
3. [인텔리제이] annotation processor 검색해서 기능 켜기
	![|450](../img/Pasted%20image%2020221102071438.png)


## 롬복의 동작 원리
- 컴파일 시점에 [애노테이션 프로세서](애노테이션%20프로세서.md) 를 사용하여 소스코드의 `AST(abstract syntax tree)`를 조작한다.
```ad-info
title: AST(abstract syntax tree)
- [AST 그림](https://javaparser.org/inspecting-an-ast/)
```

- 애노테이션 프로세서가 컴파일할때 끼어들어서 특정한 **애노테이션이 붙어있는 소스코드**(AST)를 참조해서 또 다른 소스코드를 만들어내는 기능

## 롬복의 논란거리
- JAVA의 공개되지 않은 내부 API를 사용하여 기존 소스코드를 조작한다. 공개된 API가 아니다보니 버전 호환성에 문제가 생길 수 있다.
	- [It's a total hack](http://jnb.ociweb.com/jnb/jnbJan2010.html#controversy)
	- [JAVA API - Interface Processor](https://docs.oracle.com/javase/8/docs/api/javax/annotation/processing/Processor.html)
- 그럼에도 불구하고 엄청난 편리함 때문에 널리 쓰이고 있으며 대안이 몇가지 있지만 롬복의 모든 기능과 편의성을 대체하진 못하는 현실이다

# 출처(참고문헌)
- 

# 연결문서
- [애노테이션 프로세서](애노테이션%20프로세서.md)
