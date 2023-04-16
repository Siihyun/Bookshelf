# Chapter 3 - HTTP 메시지

### HTTP 메세지의 형태

- 시작줄
    - HTTP/1.1 200 OK
- 헤더
    - Content-Type: text/plain 등 어떤 정보에 관한 메시지인지
    - Content-Length: 본문의 길이
- 본문
    - 응답 본문

### HTTP 헤더에는 어떤게 있을까?

헤더 분류

1. 일반(General) 헤더: 요청과 응답 양쪽에 모두 나타날 수 있음
    1. Date: 날짜
    2. Connection: close, keep-alive등 TCP 커넥션을 맺고 유지할지 안할지 결정
    3. Cache-Control: Cache 관련 설정
    4. Pragma: Cache 관련 설정으로, Cache-Control이 나오기 전에 주로 사용
2. 요청 헤더 : 요청에 대한 부가 정보를 제공
    1. Host: 호스트 정보
    2. User-Agent: 유저 에이전트 정보
    3. Cookie: 서버에 의해 Set-Cookie로 클라이언트에 생성된 쿠키 정보
    4. Referer: 바로 전에 머물렀던 웹 링크 주소
    5. If-Modified-Since: 해당 일 이후에 변경된 리소스 취득 요청
    6. Authorization: 인증 토큰을 서버로 보낼 때 사용하는 헤더
    7. Origin: 요청이 어느 곳에서 시작되었는지를 나타냄. 여기서 Origin이 다르면 CORS가 난다. 회피를 위해서는 응답 헤더의 Access-Control-Allow-Origin을 설정해줘야함
    8. Accept: 클라이언트 자신이 원하는 미디어 타입 및 우선순위를 알린다. Ex) Accept: ‘*/*’
    9. 예시
        
        ![예시](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b787284-3e1d-4676-aab7-c0d4a33c8c05/Untitled.png)
        
3. 응답 헤더 : 응답에 대한 부가 정보를 제공
    1. Expires: 리소스가 지정된 일시까지 캐시로써 유효함을 나타낸다. 즉, 응답 컨텐츠가 언제 만료되는지를 나타낸다. Cache-Control과 별개로 응답에 Expires라는 헤더를 줄 수 있지만, Cache-Control의 max-age가 있는 경우 이 헤더는 무시된다.
    2. Age: 캐시 응답. max-age 시간 내에서 얼마나 시간이 흘렀는지를 초단위로 알려준다
    3. Etag: 캐시 유효성 검증을 위해 사용되는 헤더. 클라이언트에 If-none-match 헤더와 관련이 있다.
    4. Allow: 해당 엔티티에 대해 서버 측에서 지원 가능한 메소드 리스트(ex: GET, POST)
    5. Access-Control-Allow-Origin: 서버에서 허용하는 오리진 목록
    6. Cache-Control: no-store(아무것도 캐시 x), no-cache(캐시를 쓰기 전에 서버에 재확인), must-revaildate(만료된 캐시만 서버에 확인), max-age(캐시 유효기간)
    7. 예시
        
        ![예시](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/08065c78-97e4-4be8-822e-812458da8c62/Untitled.png)
        
4. Entiry 헤더: 본문 크기와 콘텐츠, 혹은 리소스 그 자체를 서술
    1. Content-Type: text/html, image/png 등
    2. ConTent-Length: 30
    3. Content-Encoding: 헤당 데이터 압축 방식
    4. Content-Location: 해당 개체의 실제 위치
    5. Location: 리다이렉트때에 추가된 주소, 혹은 새로 생성된 컨텐츠 주소
    6. Last-Modified: 해당 컨텐츠가 마지막으로 수정된일시

### 상태 코드

100-199 : 정보성 상태 코드

- 100 continue : 요청의 시작 일부가 받아들여졌으며, 클아이언트는 나머지를 계속 이어서 보내야함
- 101 Switching Protocol: 클라이언트가 Upgrade 헤더에 나열한 것 중 하나로 서버가 프로토콜을 변경

200-299: 성공 상태 코드

- 200 OK: 정상
- 201 Created: 요청한 리소스가 만들어짐. 생성된 리소스와 함께, 그 리소스를 참조할 수 있는 url을 본문에 포함해야함. 로케이션 헤더에도 담아야함
    - Location헤더란?
    리소스가 리다이렉트(redirect)된 때에 이동된 주소, 또는 새로 생성된 리소스 주소를 명시한다.
    300번대 응답이나 201 Created 응답일 때 어느 페이지로 이동할지를 알려준다.
    
    새로 생성된 리소스의 경우 → HTTP 상태 코드 201 Created가 반환된다.
    
    300번대 응답의 경우 → HTTP/1.1 302 Found Location: /
    이런 응답이 왔다면 브라우저는 / 주소로 redirect한다.
- 204 No Content: 응답값에 내용이 없을때 사용

300-399: 리다이렉션 상태 코드

- 300 Multiple Choice: 클라이언트가 동시에 여러 리소스를 가리키는 url을 요청한 경우 리소스의 목록과 함께 반환
- 301 Moved Permanently: 요청한 url이 옮겨졌을때 사용. 응답은 location 헤더에 해당 리소스가 존재하고 있는 url을 포함해야한다.
- 302 Found: 301과 동일하지만 임시로 리소스가 옮겨진 경우 사용
- 303 See Other: 리소스를 다른 url에서 가져와야 한다고 말해주고자 할때 사용
- 304 Not Modified: 클라이언트는 헤더를 이용해 조건부 요청을 만들 수 있다. 캐시가 아직 유효할 경우 사용.
- 307 Temporary Redirect: 301 상태코드와 비슷하다.

400-499: 클라이언트 에러 상태 코드

- 400 Bad Request: 잘못된 요청을 보냄
- 401 Unauthorized: 인증 안됨
- 403 Forbidden : 권한없음
- 405 Method not allowed: 해당 메서드 허용되지 않음
- 406 Not Acceptable: 주어진 url에 대한 리소스에 client가 받을 수 없는 리소스가 있는 경우. client의 요청 헤더 accept 필드를 참조한다.
- 408 Request Timeout: 클라이언트 요청을 완료하기에 시간이 너무 오래 걸리는 경우 사용

500-509: 서버 에러

- 500 Internal Server Error : 서버 내부 에러에서 사용
- 502 Bad Gateway
- 504 Gateway Timeout