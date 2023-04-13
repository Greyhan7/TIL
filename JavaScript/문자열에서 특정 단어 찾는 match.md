
```javascript
문자열.match('찾는단어')
```
를 통해 특정 단어가 존재하는 지를 찾을 수 있다. 이 때 match()는 '찾는단어'를 반환한다.

- 예시 코드
```javascript
let str = '장원영 언니가 데뷔한다'
str.match('장원영') 
=> 장원영
```

### 정규 표현식을 통해 문자열 속에서 특정 문자, 숫자만 골라서 빼올 수도 있다.

```javascript
let str = '장원영의 키는 174cm이다'
str.match(\d\)
=> 174

str.match(\d\)[0]
=>1

```
