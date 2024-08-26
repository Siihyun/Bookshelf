# 애니메이션 최적화

reflow, repaint, composite 되는 속성들을 알고 이용하자

- reflow: layout부터 다시 잡아야함
  - position 관련 속성, font 관련 속성, display, width, float, height, padding, border 등
- repaint: paintin만 다시함
  - background, border-radius, border-style, box-shadow, color, outline 등
- composite만 가능한 속성
  - transform(translate, scale, rotate), opacity, cursor, perspective

# 컴포넌트 지연 로딩

- modal 같은 컴포넌트는 dynamic import를 이용하자
- click 이후 delay 방지를 위해 mouseover시 불러올수도 있음
