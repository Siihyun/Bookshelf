### 변수

- 의도를 분명히 밝혀라
- 그릇된 정보를 피하라
- 의미 있게 구분하라
  - getActiveAccount();
  - getActiveAccounts();
  - getActiveAccountInfo();
    -> 위와 같은 작명은 구분 불가능
- 자신의 기억력을 자랑하지 마라
- 한 개념에 한 단어를 사용하라
- 말장난을 하지 마라
- 해법 영역에서 가져온 이름을 사용하라(개발 용어 사용해도 된다는 의미)
  - AccountVisitor, JobQueue등
- 도메인 영역에서 가져온 이름을 사용하라
- 의미있는 맥락을 추가하라
  - firstName, lastName보다는 addrFirstName, addrLastName

코드를 읽는 이에게 명확한 의미를 전달하는 변수명을 사용하라는 느낌이었다.
남이 내 코드를 읽을때, 함수의 내부 구현을 읽지 않고도 구현 내용을 다 파악할 수 있게 만들면 참 좋을 것 같다.
