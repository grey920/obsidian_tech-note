# 날짜 : 2022-10-14 07:48

# 주제 : #java8 #memory #JVM
----
# 메모

```toc
```

```ad-note
title: tl;dr
- 
```

-   JVM의 여러 메모리 영역 중에 PermGen 메모리 영역이 없어지고 Metaspace 영역이 생김

## PermGen
- **Permenent Generation**. 클래스의 메타데이터를 담는 곳. 클래스를 로딩할 때 클래스의 이름, 클래스 내의 static 필드 등을 담음
-  Heap 영역의 일부. ( young generation -> Eden에 저장, old generation? )
- 기본값으로 `제한된 크기`를 가지고 있음(고정된 크기)
	- 클래스 로딩을 많이 하거나 동적으로 클래스를 많이 생성하는 경우PermGen 영역이 꽉 차서 문제가 될 수 있다!! -> `OutOfMemory Exception`
	- 해결1) PermGen 사이즈를 늘린다 -> 크게 해결은 안됨
	- 해결2) 클래스를 동적으로 많이 생성하는데 정리가 안되고 있는 부분의 코드를 찾아서 해결한다 -> 근본임
- -XX:PermSize=N, PermGen 초기 사이즈 설정
- -XX:MaxPermSize=N, PermGen 최대 사이즈 설정

## Metaspace
- 클래스 메타데이터를 담는 곳.
- Heap 영역이 아니라 Native메모리 영역이다. 
	- Heap엔 Eden과 Old만 있고 PermGen은 없어졌다. 그리고 새로운 Metaspace가 별도의 Native 영역에 저장된다
- 최초로 설정된 값은 있지만, 기본값으로 제한된 크기를 가지고 있지 않다. ( 필요한 만큼 계속 늘어난다. )
	- OS 메모리가 다 찰때까지 올라간다 -> 서버의 메모리가 부족하게 되어 전체 서버가 죽게 됨(!!!)
	- 메타스페이스의 최대값을 설정해줘야 한다. 만약 필요한 만큼 늘어나는 오퍼레이션도 주기 싫다면, 초기 사이즈와 최대 사이즈를 같게 설정하면 된다.
- 자바 8부터는 PermGen 관련 java 옵션은 무시한다.
- -XX:MetaspaceSize=N, Metaspace 초기 사이즈 설정
- -XX:MaxMetaspaceSize=N, Metaspace



# 출처(참고문헌)
- 

# 연결문서
- 
