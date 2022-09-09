## 날짜 : 2022-08-31 18:34

## 주제 : 
----
## 메모
```ad-note
title: tl;dr
- 
```


### Nuxt2 업그레이드하기
- dev 서버가 꺼져있는 상태에서 package lock 파일을 모두 지우고 가장 최신의 nuxt-edge를 설치한다.
```package.json
- "nuxt": "^2.15.0"
+ "nuxt-edge": "latest"
```


```ad-error
yarn install 중 'error eslint-plugin-promise@6.0.1: The engine "node" is incompatible with this module. Expected version "^12.22.0 || ^14.17.0 || >=16.0.0". Got "14.15.5"' 에러를 만남
-> nvm으로 node 버전을 14.17.0으로 업그레이드함.
```





## 출처(참고문헌)
- 

## 연결문서
- 
