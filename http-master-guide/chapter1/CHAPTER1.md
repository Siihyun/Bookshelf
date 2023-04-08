# Chapter 01 - HTTP 개관

## 1.3 리소스

웹 서버는 웹 리소스를 관리하고 제공함

### 1.3.1 미디어 타입

인터넷은 수천 가지 데이터 타입을 다루기 때문에, 데이터 타입에 대한 라벨링이 필요함.

HTTP는 웹에서 전송되는 객체 각각에 **MIME** 타입이라는 데이터 포맷 라벨을 붙임

- [MIME 타입](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

웹에서의 확장자라고 생각하면 될 것 같다. MIME 타입은 slash로 구분된 주 타입과 부 타입으로 이루어진 문자열 라벨이다. 

예시 : text/html, text/plain, image/png, 

[https://naver.com에서](https://naver.com에서) 네트워크 탭을 살펴보면, 이렇게 content-type이 담겨 오는걸 확인할 수 있다.

그렇다면 MIME Type이 http header에서 볼 수 있는 Content-Type과 같은걸까? → No

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/de0e310a-d28e-4623-85a5-588622c84aff/Untitled.png)

MIME Type은 웹보다 상위 개념인 인터넷에서 사용되는 개념이며, Content-Type은 인터넷의 하위 집합인 웹에서 사용되는 개념이다. 근데 결국 웹에서는 비슷하게 봐도 될듯하다.

### 1.3.2 URI, URL, URN

웹 서버 리소스는 각자 이름을 가지고 있기 때문에, 클라이언트는 관심 있는 리소스를 가져올 수 있음.

URI는 구조는 아래와 같음.

`scheme:[//[user[:password]@]host[:port]][/path][?query][#fragment]`

다른 예시들

```
ftp://file.fileserver.com/entries/01
http://www.ietf.org/rfc/rfc2396.txt
mailto:John.Doe@example.com
news:comp.infosystems.www.servers.unix
tel:+1-816-555-1212
telnet://192.0.2.16:80/
urn:oasis:names:specification:docbook:dtd:xml:4.1.2
http://www.ietf.org/rfc/rfc2396.txt
```

사실상 URI와 URL은 거의 동일한 개념이며, 구분하는게 큰 의미가 없는것 같아 생략한다.