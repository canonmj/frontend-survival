# TDD & BDD

## TDD(Test Driven Development)

테스트가 가능한 코드를 만든다는것은 테스트하기 쉽게 만들어진 코드를 만듬
모듈의 역할이 단순, 명확해짐 → 모듈의 크기를 줄일 수 있음 → 모듈, 계층간 커플링을 적게 만듬

## BDD(Behavior Driven Development)

TDD를 하면서 개발을 하다보면 테스트 케스에 대해서도 계속 유지보수해야하는데 귀찮기도 하고, 마감기한이 정해진 프로젝트라면 일정에 대해 압박을 받을 수도 있다. 개발리소스가 많이 들어간다..
하지만 만약 여기에서 이미 작성된 요구사항이나 기획서가 있고, 그에 맞추어 테스트 케이스를 작성하게 된다면, 위와 같은 시간에 대한 비용이 줄게된다. 바로 이게 BDD이고 프론트엔드에서 더 장점이 두드러진다.

## Given, When, Then

주어진 환경, 행위, 기대 결과

- given - 1. 본인 인증된 사용자가 2. 로그인된 상황
- when - ‘것무 정보를 입력하고, 검수 등록을 누르면’ ‘검수 정보를 미입력하고 검수 등록을 누르면’
- then - ‘등록 결과가 포함된 검수 진행 목록 화면으로 이동’ ‘검수 등록 실패 사유가 화면에 표시됨’

TDD - 테스트할 모듈의 기능을 확인하는 관점에서 작성

BDD - 시나리오 중심으로 행위를 확인하기 위한 관점에서 작성

- [카카오연사\_개념잡기 좋음](https://tv.kakao.com/channel/3693125/cliplink/414004682)
- [홀맨 TDD 연사](https://www.youtube.com/watch?v=L1dtkLeIz-M&t=175s)

## Jest

간단하고 모든것을 갖춘 자바스크립트 테스팅 프레임워크.

## 어떻게 쓸까?

RSpec이 이 분야의 원조.. 문법은 좀 다르지만 Describe-it을 지원하고 expect로 단언할 수 있다. Mocking도 다양한 레벨에서 쉽게 사용할 수 있다.

TDD - 기본적인 문법은 다음과 같다.

```jsx
test("테스트 설명", () => {
  expect("검증 대상").toXxx("기대 결과");
});
// toXxx 부분되서 사용되는 함수를 Test Matcher라고 함
```

간단히 적용한 예시

```jsx
// 테스트
function add(x: number, y: number): number {
  return x + y;
}

// 구현부
test("add 함수", () => {
  expect(add(1, 2)).toBe(3);
});
```

BDD - 시나리오 위주로 테스트 하기

```jsx
// 주어 안에 여러 시나리오를 테스트
// 많은 테스트 코드들을 그룹화할 수 있음

describe("add 함수", () => {
  it("returns sum of two numbers", () => {
    expect(add(1, 2)).toBe(3);
  });

  it("returns number", () => {
    expect(typeof add(1, 2)).toBe("number");
  });
});
```

어떤식으로 쓰냐면..

```jsx
describe('add 함수', () => {
 it('두 숫자의 합을 돌려준다', () => {
  expect(add(1, 2)).toBe(3);
 });

 context('두개의 양수가 주어질때', () => {
  it('두 개의 숫자보다 큰 값을 돌려준다', () => {
   expect(add(1, 2)).toBeLessThan(2);
  });
 })

 context('하나의 양수와 음수가 주어질때', () => {
  it('하나의 양수보다 작은 값을 돌려준다', () => {
   expect(add(1, -2)).toBeLessThan(1);
  });
 })
});

// PASS 결과
add 함수는
 ✔️ 두 숫자의 합을 돌려준다
 두개의 양수가 주어질때
  ✔️ 두 개의 숫자보다 큰 값을 돌려준다
 하나의 양수와 음수가 주어질때
  ✔️ 하나의 양수보다 작은 값을 돌려준다
```

상품에 할인쿠폰이 적용되었을때 같은 경우를 테스트하기 좋다고 한다👍
