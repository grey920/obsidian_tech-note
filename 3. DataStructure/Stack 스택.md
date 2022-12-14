## 날짜 : 2022-08-30 06:46

## 주제 : #자료구조 #선형구조 
----
## 메모
> 출/입구가 하나뿐인 LIFO 구조. 뚜껑식 김치 냉장고


### 규칙
- 처음 넣은 것은 맨 아래 바닥에 깔린다.
- 바닥에 있는 것을 꺼내려면 위에 쌓인 것들을 모두 치우는 수 밖에 없다. (LIFO)

### 조작
- puxh(x): 스택의 탑에 요소 x를 추가
- pop(x): 스택의 탑에 있는 요소 추출
- isEmpty(): 스택이 비어 있는지 확인
- isFull(): 스택이 꽉 찼는지 확인

### 이 구조는 왜 필요할까? 
- 뭔가를 되돌아가기 위한 구조 (!!!)
	- 마치 헨젤과 그레텔처럼 흔적을 하나씩 흘려놓고 왔던 길을 되돌아가기 위해 추적하려면 가장 마지막부터 되돌아가면 된다. 
- 오래 사용할 데이터를 저장하기 보다는, 임시 데이터를 다뤄야 하는 경우 유용하다. 



## 출처(참고문헌)
- 널널한 개발자 - 넓고 얕은 자료구조와 알고리즘2 : https://youtu.be/Jippp2mMB2U
- 프로그래밍 대회 공략을 위한 알로기즘과 자료 구조 입문 - 4장 자료구조 (68p)

## 연결문서
- [[Queue 큐 (대기열)]]
