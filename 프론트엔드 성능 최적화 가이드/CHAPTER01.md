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
