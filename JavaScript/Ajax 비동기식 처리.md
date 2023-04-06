# Ajax 비동기식 처리

### Ajax란?
* Asynchronous Javascript And XML 의 약자. 말 그대로 자바 스크립트 라이브러리 중 하나다.
* 자바 스크립트를 통해 서버에 데이터를 요청할 때, 전체 페이지를 고치지 않고 변경이 필요한 부분만 고치는 기법이다.
* 화면 전환 없이 클라이언트와 서버 간의 데이터를 전송하여 말끔한 화면 경험을 하게 해준다.
* 또한 페이지 갱신 없이 데이터 통신을 하기 때문에 웹페이지의 속도가 향상되며, 별도의 플러그인이 필요하지 않다는 장점도 있다.

### 동기식, 비동기식 방식
![image](https://user-images.githubusercontent.com/99037697/230319132-c6327c48-2d1f-465c-85a6-cc016a65a249.png)

- 동기식은 요청 후 응답이 있어야만 다음 동작이 이루어지는 반면, 비동기식에선 응답과 관계 없이 동작이 이루어진다.
- 기본적으로 Ajax 기본 옵션에서 비동기식으로 되어있지만, async 옵션을 false로 설정하면 동기식으로 변화시킬 수 있다.

```javascript
function Ex(){
  $.ajax({
    data : data,
    url : url,
    async : false,
    success : function(){
     ~~~~
    }
  })
}
```
