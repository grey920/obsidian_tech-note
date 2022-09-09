## 날짜 : 2022-08-30 20:45

## 주제 : #자료구조 #알고리즘 
----
## 메모
> 빅오는 알고리즘의 성능을 수학적으로 표현한 표기법

- [[Time complexity 시간복잡도]]와 공간 복잡도를 표현한다
- 알고리즘의 실제 러닝타임을 표현하는게 아니다. 
- 데이터나 사용자의 증가율에 따른 알고리즘의 성능을 예측하는 것이 목표이다. -> `상수와 같은 숫자들은 모두 1이다. `
	```ad-faq
	처음엔 이게 무슨 뜻인지 몰랐는데, 강의를 듣고나니 `증가율을 예측한다`는 말이 핵심이었다. 
	**상수**는 `고정된 값만큼만 증가율에 영향`을 미치므로 증가하지 않는 숫자는 신경쓰지 않아서 1로 생각하는 것이다. 
	```

### 빅오 표기법의 몇가지 예시
![[Pasted image 20220830220023.png|450]]
[Big-O Complexity Chart](https://www.bigocheatsheet.com/)
```ad-note
title: 빅 오 표기법의 속도
O(1) < O(logN) < O(N) < O(NlogN) < O(N^2) < O(N^3) < O(2^N) < O(N!)
- 오른쪽으로 갈수록 속도가 커진다.( 성능이 더 안좋음 )
- 알고리즘의 경우 최소 O(N^2)과 비슷한 수준이나 더 빠르게 동작하도록 설계되어야 한다.
```


1. O(1) 알고리즘 : constant time
	- 입력된 데이터 크기에 상관없이 `언제나 일정한 시간이 소요`되는 알고리즘
	- ![[Pasted image 20220830205239.png|300]]
	- data가 증가해서 성능에 변함이 없다. 
2. O(n) 알고리즘: linear time
	- 입력 데이터에 비례해서 처리 시간이 걸리는 알고리즘
	- ![[Pasted image 20220830205510.png|300]]
 3. O(n^2) 알고리즘: quadratic time
	 ```java
	 F( int[] n ) {
		 for i = 0 to n.length
			 for j = 0 to n.length
				 print i + j; 
	 }
	 ```
	- n개의 loop를 돌면서 그 안에 n번의 loop를 또 돌때
	- ![[Pasted image 20220830210034.png|300]]
	- 데이터 n이 +1이 되면 아래 한 줄, 오른쪽 한 줄이 함께 늘어난다. 즉, 데이터가 커질수록 처리 시간에 대한 부담도 함께 커진다. 
	- ![[Pasted image 20220830210106.png|300]]
	- 뒤로 갈수록 하나 추가할 때 마다 수직상승한다.
4. O(nm) 알고리즘: quadratic time
	- O(n^2)와 비슷하지만, 이건 m을 n만큼 돌린다. 
	```java
	 F( int[] n, int[] m ) {
		 for i = 0 to n.length
			 for j = 0 to m.length
				 print i + j; 
	 }
	```
	- ![[Pasted image 20220830210814.png|300]]
	- n은 엄청 크고 m은 작을 수 있기 때문에 **변수가 다르다면** `빅오 표기법에도 반드시 다르게 표기`해야 한다. 
5. O(n^3) 알고리즘: polynomial / cubic time
	- n을 세제곱한다. loop를 삼중으로 돌린다
	```java
	 F( int[] n ) {
		 for i = 0 to n.length
			 for j = 0 to n.length
				 for k = 0 to n.length
					 print i + j + k; 
	 }
	```
	- ![[Pasted image 20220830211243.png|300]] 
	- 아무래도 높이가 추가되니, 데이터가 늘어날수록 처리 시간이 급격하게 상승한다.
	- ![[Pasted image 20220830211427.png|300]]
6. O(2^n) 알고리즘: exponential time
	- 피보나치 수열 - 앞의 두 수를 계속 더한 값을 잇는다. 
	- ![[Pasted image 20220830211745.png|400]]
	- 재귀함수를 이용하여 코드를 구현. 매번 함수를 호출할 때 마다 두번씩 또 호출을 하게된다. 이를 트리의 높이만큼 반복한다.
	```java
	F( n, r ) {
		if ( n <= 0 ) return 0;
		else if ( n == 1 ) return r[n] = 1;
		return r[n] = F( n-1, r ) + F( n-2, r );
	}
	```
	- ![[Pasted image 20220830212211.png|400]]
	- ![[Pasted image 20220830212323.png|300]] 
	- 시간복잡도로 보면 n의 세제곱보다도 데이터의 증가에 따라 처리시간이 훨씬 증가한다.
7. O(m^n) 알고리즘: exponential time
	- 이건 6.의 O(2^n) 알고리즘에서 2 대신 m이라고 생각하면 된다.
8. O(log n) 알고리즘: 대표적으로 binary search  ( 이진검색 )
	- 한 번 처리가 진행될 때마다 검색해야 할 데이터의 양이 절반씩 줄어드는 알고리즘
	```java
	// s: 시작점 0, e: 배열의 마지막
	F( k, arr, s, e ) {
		if ( s > e ) return -1;
		m = ( s + e ) / 2; // 범위의 중간값 찾기
		if ( arr[m] == k ) return m;
		// 찾는 값이 중간값보다 작으면 중간값 바로 앞까지 범위를 재조정해서 다시 호출
		else if ( arr[m] > k ) return F( k, arr, s, m-1 );
		// 찾는 값이 중간값보다 크면 중간값 바로 다음부터 마지막까지 범위를 재조정해서 다시 호출
		else return F( k, arr, m+1, e );
	}
	```
	- ![[Pasted image 20220830213233.png|300]]
	- 데이터가 증가해도 성능이 크게 차이나지 않는다.
9. O( sqrt(n) ) 알고리즘 (제곱근. square root )
   - ![[Pasted image 20220830213501.png|400]]
   - n으로 정사각형을 만들었을때 가장 윗줄만 뽑은게 squre root


### 빅오에서 중요한 점!! -> **상수**는 과감하게 버린다!
- 예를 들어 아래의 코드같은 경우, 시간복잡도는 O(2n)이다. 빅오 표기법에서는 O(2n)은 O(n)이다.
	```java
	F( int[] n ) {
		for i = 0 to n.length
			print i
		for i = 0 to n.length
			print i
	}
	```
- 빅오 표기법의 목표는 장기적으로 데이터가 증가함에 따른 `처리시간의 증가율을 예측`하기 위해 만들어진 표기법이기 때문에 **상수**는 `고정된 숫자`이므로 증가율에 영향을 미칠 때 언제나 고정된 상수만큼만 영향을 미치기 때문에 여기서 **증가하지 않는 숫자는 신경쓰지 않는다**



## 출처(참고문헌)
- 엔지니어대한민국:  https://youtu.be/6Iq5iMCVsXA

## 연결문서
- 
