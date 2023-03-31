### thymeleaf에서 날짜 데이터 포맷 설정하기

#### Date 포맷일 때 -> #dates.format(날짜, 형식)
```HTML
 <td th:text="${#dates.format('20230331 00:00:00','yyyy-MM-dd')}"></td>
```

#### String 포맷일 때 -> #temporals.format(날짜, 형식)
```HTML
 <td th:text="${#temporals.format('20230331','yyyy-MM-dd')}"></td>
```

참고 (thymeleaf 공식 깃허브)
https://github.com/thymeleaf/thymeleaf-extras-java8time
