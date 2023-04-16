### 정의
원격 api를 간편하게 호출할 수 있는 Ajax 통신 기술의 일.

Ajax에서 활용하는 XMLHttpRequest 와 달리 Promise 기반으로 되어 있어 더욱 간편하다.

### 기본 문법

#### GET 방식
```javascript
fetch(url, [options])
  .then((response => response.json())) // 응답 결과를 json으로 바꾸기
  .then((data => {
    console.log(data) // 응답 결과를 출력
  }))
  .catch(err =>{
    console.log('에러', err)
  })
```

#### Post 방식
```javascript
fetch(url, {method:'post'})
  .then((response => response.json())) // 응답 결과를 json으로 바꾸기
  .then((data => {
    console.log(data) // 응답 결과를 출력
  }))
  .catch(err =>{
    console.log('에러', err)
  })
```

