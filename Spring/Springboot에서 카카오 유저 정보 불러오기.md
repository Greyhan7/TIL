## Springboot에서 카카오 유저 정보 불러오기

<hr>


### 0. 구조 및 순서

<img src="https://user-images.githubusercontent.com/99037697/233779156-3f0667cf-0a85-409f-9e59-ca54bb1c0e69.png" hegiht="700" width="500"></img>

1) 유저가 만든 웹사이트에서 카카오 로그인과 연결된 버튼 클릭.
2) 카카오 로그인 창 호출.
3) 동의화면을 통해 카카오 유저 정보를 받기 위한 token의 인가 코드 요청.
4) 카카오로부터 받은 코드를 통해 token 요청.
5) 토큰을 카카오 서버에 보내 이에 맞는 사용자 정보를 돌려줌.
6) json으로 된 사용자 정보를 알맞게 변형하여 사용.


- 쉽게 말하자면 로그인 -> token을 위한 코드 받기 -> 코드를 갖고 token 받기 -> token을 갖고 사용자 정보 받기 의 과정이다.

<hr>


### 1. 카카오 Developer에서 기본 설정하기

https://developers.kakao.com/

- 어플리케이션 설정해서 REST API 랑 Redirect URI 따기 (앞으로 REST API는 API로, Redirect URI는 URI로 말함)
- 동의항목 설정하기 (카카오 닉네임, 성별, 이메일 등을 설정 가능)

<hr>

### 2. 카카오 로그인 호출하는 버튼 만들기

```html
<a id="kakao-login-btn" href="https://kauth.kakao.com/oauth/authorize?client_id=위에서 받은 API&redirect_uri=위에서 설정한 URI&response_type=code">
  <img src="https://k.kakaocdn.net/14/dn/btroDszwNrM/I6efHub1SN5KCJqLm1Ovx1/o.jpg" width="222" alt="카카오 로그인 버튼" />
</a>
```
<hr>

### 3. Controller에서 code 받기
- 위의 URI 주소에 받는 컨트롤러 메소드 만든 후, @RequestParam 으로 String 형태의 코드를 받는다.
- 이 코드로 사용자 정보를 갖고 올 수 있는 token에 대한 인가를 받을 수 있다.

```java
@GetMapping("/oauth/kakao")
    @ResponseBody
    public void kakaoLogin(@RequestParam String code){
        String access_token = cs.getKakaoToken(code);
    }
```

<hr>

### 4. 위에서 받은 code로 Token 받기 위해 service 에서 메소드 작성

```java
 // 카카오 계정 토큰 발급
    public String getKakaoToken(String code){
        String access_token = "";
        String refresh_token = "";
        String client_id = 위에서 받은 API
        String post_url = 위에서 설정한 URI
        String request_url = "https://kauth.kakao.com/oauth/token";

        try {
            URL url = new URL(request_url);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();

            //POST 요청을 위해 기본값이 false인 setDoOutput을 true로
            connection.setRequestMethod("POST");
            connection.setDoOutput(true);

            //POST 요청에 필요로 요구하는 파라미터 스트림을 통해 전송
            BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(connection.getOutputStream()));
            StringBuilder sb = new StringBuilder();

            sb.append("grant_type=authorization_code");
            sb.append("&client_id="+client_id);
            sb.append("&redirect_uri="+post_url);
            sb.append("&code=" + code);
            bw.write(sb.toString());
            bw.flush();

            //결과 코드가 200이라면 성공
            int responseCode = connection.getResponseCode();
            System.out.println("responseCode : " + responseCode);

            //요청을 통해 얻은 JSON타입의 Response 메세지 읽어오기
            BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line = "";
            String result = "";

            while ((line = br.readLine()) != null) {
                result += line;
            }
            System.out.println("response body : " + result);

            // gson 라이브러리를 통해 json 읽기
            JsonParser parser = new JsonParser();
            JsonElement element = parser.parse(result);
            access_token = element.getAsJsonObject().get("access_token").getAsString();
            refresh_token = element.getAsJsonObject().get("refresh_token").getAsString();

            System.out.println("access_token : " + access_token);
            System.out.println("refresh_token : " + refresh_token);
            br.close();
            bw.close();

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        return access_token;
    }
```

- access_token을 얻었기 때문에 이를 통해 사용자 정보를 불러오면 된다.

<hr>

### 5. token을 통해 사용자 정보 얻기

- 나는 일단 HashMap에 사용자 정보 (닉네임, 이메일) 를 담아 반환하였다. 나중에 회원가입 할 때 이 정보를 사용하면 된다.

```java
  public HashMap<String, String> getKakaoUser(String access_token){
        String request_url = "https://kapi.kakao.com/v2/user/me";
        HashMap<String, String> userInfo = new HashMap<>();

        try {
            URL url = new URL(request_url);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();

            //POST 요청을 위해 기본값이 false인 setDoOutput을 true로
            connection.setRequestMethod("POST");
            connection.setDoOutput(true);
            connection.setRequestProperty("Authorization", "Bearer " + access_token);

            //결과 코드가 200이라면 성공
            int responseCode = connection.getResponseCode();
            System.out.println("responseCode : " + responseCode);

            //요청을 통해 얻은 JSON타입의 Response 메세지 읽어오기
            BufferedReader br = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line = "";
            String result = "";

            while ((line = br.readLine()) != null) {
                result += line;
            }
            System.out.println("response body : " + result);

            // gson 라이브러리를 통해 json 읽기
            JsonParser parser = new JsonParser();
            JsonElement element = parser.parse(result);

            // json 객체 element를 분해하여 원하는 정보를 가져오기
            boolean hasEmail = element.getAsJsonObject().get("kakao_account").getAsJsonObject().get("has_email").getAsBoolean();
            String nickname = element.getAsJsonObject().get("kakao_account").getAsJsonObject().get("profile").getAsJsonObject().get("nickname").getAsString();
            String email = "";
            if (hasEmail) {
                email = element.getAsJsonObject().get("kakao_account").getAsJsonObject().get("email").getAsString();
            }
            
            userInfo.put("nickname", nickname);
            userInfo.put("email", email);
            br.close();

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        return userInfo;
    }
```

<hr>

- 코드 작성이 어렵진 않은데 원리를 이해하는 게 어렵다.
- API 활용을 위한 Http 프로토콜에 대해선 다음 링크를 참조. 나중에 정리하자.
    - https://blueyikim.tistory.com/2199
