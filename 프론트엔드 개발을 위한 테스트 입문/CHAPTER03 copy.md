# 목 객체

## 4.1 목 객체를 사용하는 이유

- 테스트는 실제 실행 환경과 유사할수록 재현성이 높지만, 실행 시간이 너무 많이 걸리거나 환경 구축이 어려운 경우가 있음
- 실패하는 경우 테스트하기 쉽지 않음

### 스텁

- 의존 중인 컴포넌트의 대역
- 정해진 값을 반환하는 용도

### 스파이

- 함수나 메서드의 호출 기록
- 호출된 횟수나 인자 기록

## 4.3 웹 API 목 객체 기초

- 테스르를 할 때는 웹 API 관련 코드를 스텁으로 대체하여 테스트를 작성한다.

스텁 대체 예시

```jsx
jest.mock("/greet", () => ({
  sayGoodBye: (name: string) => `Good Bye. ${name}`,
}));

test("인사말을 반환한다", () => {
  expect(greet("Taro").toBe("Good Bye. Taro."));
});
```

스파이 대체 예시

```jsx
import * as Fetchers from "./fetchers";
jest.mock("./fetchers");
jest.spyOn(Fetchers, getMyProfile);
```

---

목 데이터 만드느라 고통받았던 기억이 떠오르던 챕터였습니다ㅎㅎ;;
저자가 말한것 처럼 jest는 stub(mock), spy의 개념이 혼용되어 쓰이고 있는 것 같아요. 원래 구분되는 개념이란것도 몰랐어서 이 부분 좀 재밌었습니다ㅎㅎ
