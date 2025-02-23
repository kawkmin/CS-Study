# HTTP status code

<br>

> [HTTP 참고](https://github.com/jmxx219/CS-Study/blob/main/Network/HTTP.md)
  
  
- HTTP 상태 코드는 클라이언트의 요청에 대한 서버에서 반환되는 코드를 말함 
- 상태 코드에 따라 요청의 성공/실패 여부를 세 자릿수로 구분함
  - `1xx (조건부 응답)`
  - `2xx (성공)`
  - `3xx (리다이렉션)`
  - `4xx (클라이언트 오류)`
  - `5xx (서버 오류)`

<br>

## 1xx (Informational) : 조건부 응답

- 임시 응답으로, 클라이언트가 보낸 요청을 서버가 받았으니 클라이언트는 작업을 계속 진행하라는 의미
- HTTP/1.0 이후 거의 쓰이지 않음

|     상태코드     |    이름     |                         의미                          |
|:------------:| :---------: |:---------------------------------------------------:|
|     100      |     Continue      | 클라이언트가 서버로 보낸 요청에 문제가 없으니 다음 요청을 이어서 보내도 된다는 것을 의미함 |
|     101      |   Switching Protocols    |     프로토콜 전환하라는 의미. 현재 HTTP 1.1이 최신이므로 사용할 일이 없음     |
|     102      |  Processing   |          서버가 처리 중이므로 제대로된 응답을 알려줄 수 없다는 의미          |


<br>
<br>

## 2xx (Successful) : 성공

요청이 서버에서 정상적으로 수신되었고, 성공적으로 처리되었다는 의미

| 상태코드 |    이름     |                                            의미                                            |
| :------: | :---------: |:----------------------------------------------------------------------------------------:|
|   200    |     OK      |                       요청이 성공적으로 수행되었음을 의미함. 주로 GET 요청에 대한 응답으로 사용                        |
|   201    |   Create    |                요청이 성공했고, 새로운 리소스가 생성되었음을 의미함. 주로 POST와 PUT 요청에 대한 응답으로 사용                |
|   202    |  Accepted   | 요청은 성공했으나, 서버가 아직 요청을 완료하지 못했음을 의미함. 배치 처리와 같이 요청 접수 후 일정 시간이 지난 후에 요청을 처리하는 경우의 응답으로 사용 |
|   204    | No Contents |          요청이 성공적으로 수행되었고, 응답 payload에 보낼 데이터가 없음을 의미함. 주로 DELETE 요청에 대한 응답으로 사용          |


<br>

### 200 vs 201

- 두 코드 모두 성공적인 요청을 의미하는 2xx 계열이지만 서로 다른 맥락으로 사용됨
- 200 OK 코드는 성공적인 응답에 사용되며, 클라이언트에서 요청된 리소스가 존재하고 반환되었음을 나타냄 
- 반면 201 Created 코드는 새 리소스가 성공적으로 생성되었음을 나타내며, 클라이언트가 찾을 수 있게 위치에 대한 정보도 제공함


<br>
<br>

## 3xx (Redirection) : 리다이렉션 완료

클라이언트가 요청을 완료하기 위해 추가 작업을 수행해야 함을 의미(리다이렉션)

| 상태코드 |        이름        |                             의미                              | 리다이렉션 시, 메소드 변경 유무 |
|:----:|:----------------:|:-----------------------------------------------------------:|:------------------:|
| 300  | Multiple Choice  |                  요청한 URI에 여러 리소스가 존재함을 의미함                  |                    |
| 301  | Move Permanently |               요청한 리소스의 URI가 영구적으로 변경되었음을 의미함                |   메서드 GET으로 변경됨    |
| 302  |    Found     |               요청한 리소스의 URI가 일시적으로 변경되었음을 의미함                |    메서드 GET으로 변경될 수 있음    |
| 303  |    See Other     |           요청한 리소스를 다른 URI의 GET 요청을 통해 얻어야 함을 의미함            |   메서드 GET으로 변경됨    |
| 304  |   Not Modified   | 리소스가 수정되지 않았음을 의미함. 클라이언트는 서버로부터 리소스를 재전송받지 않고 캐싱된 리소스를 사용함 |                    |
| 307  |   Temporary Redirect   |   302와 기능은 같지만, HTTP 요청 메서드와 본문을 유지한 채 리다이렉션 요청이 필요함을 의미함   |         유지         |
| 308  |   Permanent Redirect   |   301과 기능은 같지만, HTTP 요청 메서드와 본문을 유지한 채 리다이렉션 요청이 필요함을 의미함   |         유지         |


<br>

###  리다이렉션

- 클라이언트가 요청한 URL에 대해 다른 URL을 다시(re) 지시(direct)하여 다른 주소로 이동할 수 있게 하는 기술
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동함

<br>

#### 리다이렉션 종류
- 영구 리다이렉션(Permanent) - `301`, `308`
  - 특정 리소스의 URI가 영구적으로 이동
  - ex) `/members` ➜ `/users`, `/event` ➜ `/new-event`,
- 일시 리다이렉션(Temporary) - `302`, `303`, `307`
  - 특정 리소스의 URI이 일시적으로 이동
  - ex) 주문 완료 후 주문 내역 화면으로 이동
  - ex) `PRG(Post/Redirect/Get)`
    - POST로 주문 후에 웹 브라우저를 새로고침하면 다시 요청이 되기에 중복 주문이 될 수 있음(문제)
    - 이때 `Post`로 주문 후에 주문 결과 화면을 `Get`으로 리다이렉트 요청함으로써 새로 고침해도 결과 화면을 GET으로만 조회하게 할 수 있음
- 특수 리다이렉션(Special) - `300`, `304`
  - `300 Multiple Choices` : 안씀
  - `304 Not Modified`: 클라이언트는 로컬 PC에 저장된 캐싱된 리소스를 사용함

<br>

#### 영구 리다이렉션(`301 vs 308`)

- 두 코드 리소스의 URI가 영구적으로 이동되어 새로운 URI로 리다이렉션을 의미하는 3xx 계열
  - `301 Moved Permanently`은 리다이렉트 요청 시 메서드가 **GET** 으로 변하고 본문이 제거될 수 있음(MAY)
  - `308 Permanent Redirect`은 리다이렉트 시 HTTP 메서드와 본문(body)을 유지함. 처음 **POST**를 보내면 리다이렉트도 **POST**를 유지함
- URI가 변경되면 내부적으로 전달해야 하는 데이터가 변경될 수 있기 때문에 **POST**를 유지하는 것보단 **GET**을 사용하여 새로운 URI를 확인시켜주는 것이 안전함
  - 따라서 `308` 보다는 `301`을 더 많이 사용함

<br>

#### 일시적 리다이렉션(`302 vs 303 vs 307`)

- 세 코드 모두 리소스의 URI가 일시적으로 변경됨을 의미함
- 세 코드의 차이점은 리다이렉트 후 요청 메서드의 변경 유무
  - `302 Found`: 리다이렉트 시, 요청 메서드가 GET으로 변하고 본문이 제거될 수 있음(MAY)
  - `303 See Other`: 302와 기능은 같고, 리다이렉트 시 요청 메서드가 GET으로 변경됨
  - `307 Temporary Redirect`: 302와 기능은 같고, 리다이렉트 시 요청 메서드와 본문울 유지함(요청 메서드를 변경하면 안됨, MUST NOT)
- 처음 302의 스펙 의도는 HTTP 메서드를 유지하는 것이였지만 대부분의 브라우저들이 GET으로 바꾸게 됨
  - 그래서 애매한 302를 대신하고자 명확한 303과 307이 등장하게 되었음


<br>
<br>

## 4xx (Client Error) : 요청 오류

클라이언트 요청이 잘못되었음을 의미

| 상태코드 |        이름        |                               의미                               |
| :------: | :----------------: |:--------------------------------------------------------------:|
|   400    |    Bad Request     |     클라이언트가 잘못된 요청을 보냄을 의미함. 주로 요청 구문, 메시지 등의 문법 오류로 인해 발생      |
|   401    |    Unauthorized    |            인증되지 않은 사용자가 인증이 필요한 리소스를 요청했음을 의미함(비인증)            |
|   403    |     Forbidden      |          인증 자격은 증명되었으나, 접근할 권한을 가지고 있지 않음을 의미함(권한 x)           |
|   404    |     Not Found      |                       요청 URI에 대한 리소스 존재X                       |
|   405    | Method Not Allowed |                     요청한 리소스가 존재하지 않음을 의미함                      |
|   406    |   Not Acceptable   | 요청이 허용되지 않은 메서드를 사용했음을 의미함. 서버에서 해당 요청 HTTP 메서드에 대해 기능을 제한/금지함 |
|   408    |  Request Timeout   |                   요청에 응답하는 시간이 너무 오래 걸림을 의미함                   |
|   409    |      Conflict      |                클라이언트의 요청이 서버의 현재 상태와 충돌되었음을 의미함                |
|   429    |  Too Many Request  |               클라이언트가 지정된 시간동안 너무 많은 요청을 보냈음을 의미함               |


<br>

### 401 vs 403

- `401 Unauthorized` 응답 코드는 클라이언트 요청에 유효한 자격 증명을 제공해야함을 뜻함
  - 즉, 401이 반환되었다는 것은 리소스에 대한 요청에 유요한 자격 증명이 없음을 의미함
  - ex) 로그인이 안된 상태
- `403 Forbidden` 응답 코드는 클라이언트가 유요한 자격 증명을 제시했지만, 요청한 리소스에 접근할 수 없음을 의미함
  - 자격 증명을 제시했기 때문에 403의 경우, 서버는 클라이언트가 누구인지 알고 있음
  - ex) 로그인은 했지만, 권한이 없는 상태


<br>
<br>

## 5xx (Server Error) : 서버 오류

요청을 받은 서버에서 오류가 발생했음을 의미

| 상태코드 |         이름          |                                         의미                                          |
| :------: | :-------------------: |:-----------------------------------------------------------------------------------:|
|   500    | Internal Server Error |              서버 내부 오류가 발생하여 응답할 수 없음을 의미함. 오류가 발생했지만 처리 방법을 알 수 없는 상태               |
|   501    |      Not Implemented      |                      클라이언트 요청에 대한 서버의 응답 수행 기능이 없음을 의미함(구현 x)                       |
|   502    |      Bad Gateway      | 게이트웨이 또는 프록시 역할을 하는 서버가 액세스하고 있는 다른 서버로부터 잘못된 응답을 수신했음을 의미함. 서버의 부모 서버에서 오류가 발생한 경우 |
|   503    |  Service Unavailable  |                    서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음을 의미함                    |
|   504    |    Gateway Timeout    |            게이트웨이 또는 프록시 역할을 하는 서버가 액세스하고 있는 다른 서버로부터 적시에 응답을 받지 못했음을 의미함            |



<br>
<br>

### Ref

- [HTTP 상태 코드(1XX ~ 5XX) 종류 총정리](https://inpa.tistory.com/entry/HTTP-%F0%9F%8C%90-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C-1XX-5XX-%EC%B4%9D%EC%A0%95%EB%A6%AC%ED%8C%90-%F0%9F%93%96)
