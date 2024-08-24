# Chapter 01 - 블로그 서비스 최적화

블로그 서비스의 LightHouse 지표를 직접 개선하는 과정을 보여주며 최적화를 설명

## LightHouse 주요 지표

- fcp
  - 페이지가 진입한 후, 렌더링되는 속도
    ⇒ html 파일을 다 받아온 이후 paint되는 속도로, ttfb와는 다르다.
- lcp
  - 가장 늦게 뜬 화면이 보이는 속도
  - 보통은 이미지가 많고, api latency가 큰 경우 api 관련 데이터기도 함
- ttfb
  - 실제 서버와의 물리적 거리
  - **서버에서 html을 준비하는 속도**
- tbt
  - total blocking time으로, TTI - FCP의 값을 지닌다.
  - 유저 입력이 blocking된 시간
- cls
  - cumulative layout shift로, 의도치 않은 레이아웃 이동
  - 주로 이미지나 새로운 영역이 들어오며 변하는 현상

web dev 내용 추가

- 일반적인 html 성능 개선사항
  - 리디렉션 최소화
  - html 응답 캐시
  - 서버 응답 개선(ttlb)
  - cdn 사용(ttlb)
- 중요 경로(critical path) 이해
  - 브라우저가 렌더링을 하기 위해 꼭 필요한 최소한의 요소들
  - 기본적으로 css와 javascript는 렌더링 블락킹 요소다
    - inline style, media요소나, preload로 된 css, async나 defer가 붙어있는 js는 렌더링 블로킹 요소가 아님
  - 렌더링 차단 요소와 파싱 차단 요소를 나눠서 보자
    - <script> 태그는 파싱 차단 요소다.
- [리소스 로드 최적화](https://web.dev/learn/performance/optimize-resource-loading?hl=ko#avoid_css_import_declarations)
  - 렌더링 차단 리소스
    - js, css 파일 로드
  - 파서 차단 리소스
    - async, defer script 없이 이용하면 파서를 차단
    - 이런 parser block script는 render block resource(ex: css)가 도착한 후에야 실행 가능함. script에서 css 수정(예: **`element.getComputedStyle()`** 사용)을 할 수 있기 때문
  - preload scanner
    - html 파싱이 블락되었다고 하더라도, 브라우저는 preload scanner를 통해 가능한 다음에 load 해놓을 항목을 찾으려고 시도
      - 예를 들어 미리 로드 스캐너를 사용하면 브라우저에서 HTML 파서가 차단된 경우에도 `<img>` 요소에 지정된 리소스 CSS와 JavaScript 같은 리소스를 가져오고 처리할 수 있습니다.
    - 그래서 preload scanner가 다음에 load할 항목을 찾을 수 있게 해주는건 꽤 중요한데, 아래의 항목은 찾을 수 없다.
      - `background-image` 속성을 사용하여 CSS에 의해 로드된 이미지 - 이 이미지 참조는 CSS에 있으며 미리 로드 스캐너가 검색 불가
      - 삽입된 `<script>` 요소 - 마크업 형식으로 동적으로 로드된 스크립트 또는 [동적 `import()`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/import)를 사용하여 로드된 모듈을 사용하여 DOM에 삽입할 수 있습니다.
      - 자바스크립트를 사용하여 클라이언트에서 렌더링된 HTML
      - CSS `@import` 선언
  - css에서 @import 형식으로 가져오는 것도 피하는게 좋음. css 내부에서 또 파일을 호출해야 하므로 network chaning이 일어남(병렬 불가)
    script - async/defer/module
  - defer - parsing block x, parsing 끝나고 실행
  - async - parsing block x, 이후 parsing block 후 실행
  - module - defer와 같이 동작
  - module async - async처럼 동작
  - defer async - 하위 호환성
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/fc16a6ac-6598-4a40-9054-f12bc3705e18/eb2df4b5-6ed4-4834-b2fe-65ec04b5a807/image.png)
- 리소스 힌트로 브라우저 지원
  - preconnect
    `<link rel="preconnect" href="https://fonts.googleapis.com">` - dns query, tcp connection, tls connection등을 맺는다.
  - dns-prefetch
    - dns query만 미리 수행. preconnect 보다 훨씬 비용이 덜 든다
  - preload
    - 미리 불러오는 것
  - prefetch
    - 얘도 미리 불러오는 건데, 좀 다른건 얘는 안 쓰일 수도 있음. 하지만 높은 확률로 쓰이는 애들
    - 우선순위가 낮게 fetch됨
