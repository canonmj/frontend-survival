# Jest

## Jest 🎪

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
