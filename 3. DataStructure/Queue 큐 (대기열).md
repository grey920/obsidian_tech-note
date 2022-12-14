## 날짜 : 2022-08-30 06:54

## 주제 : #자료구조 #선형구조 
----
## 메모
> 출/입구가 양끝에 각각 따로 존재하는 FIFO 구조. 버스 대기 줄 또는 정수기 종이컵. 대기열이라고도 부른다. 

### 규칙
- 가장 먼저 들어간 자료를 먼저 꺼내는 선입선출 규칙에 따라 자료를 관리한다. 
- enqueue된 시간이 가장 오래된 요소부터 추출된다. ( == dequeue는 추가된 순서대로 요소를 추출할 때 활용한다. )

### 조작
- enqueue(x): 큐 끝에 x 추가
- dequeue(): 큐 가장 앞의 요소를 추출
- isEmpty(): 큐가 비어있는지 확인
- isFull(): 큐가 꽉 찼는지 확인

### 이 구조가 왜 필요할까?
- 자료를 도착한 순서대로 처리하고 싶은 경우 사용한다. 
- 스택과 비슷하게 간결하게 임시 데이터를 다룰 때 유용하다. 다만 데이터를 처리하는 순서가 스택과 다르다. 

```ad-faq 생각해 볼 문제
비슷해보이지만 버스를 타려고 줄을 선 것과 은행에서 기다리는 것은 어떤 차이가 있을까?
- 은행은 창구가 여러개지만, 버스는 하나이다. 따라서 버스는 순서대로 처리(순차처리)되지만 은행은 처리가 동시에 (병렬처리) 처리가 되는 구조이다. 
```


## 출처(참고문헌)
- 널널한 개발자 - 넓고 얕은 자료구조와 알고리즘2 : https://youtu.be/Jippp2mMB2U
- 프로그래밍 대회 공략을 위한 알로기즘과 자료 구조 입문 - 4장 자료구조 (69p)

## 연결문서
- 
