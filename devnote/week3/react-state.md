# React State

## React state란?

- State는 컴포넌트 내에서 지속적으로 변경이 일어나는 값을 관리하기 위해 사용한다.
- 개발자가 의도한 동작에 의해 변할 수도 있고 사용자의 입력에 따라 새로운 값으로 변경될 수도 있다.
- “Re-rendering” 값이 변경되고 재 렌더링이 필요한 경우에 React가 자동으로 변경된 부분을 렌더링 한다.

### state 값은 직접 변경하지 않는다

```jsx
import { useState } from 'react';

function Example() {
    const [count, setCount] = useState(0);
    return (
        <div>
            <p>버튼을 {count}번 눌렀습니다.</p>
            <button onClick={() => {count = count + 1;}>클릭</button> // 이렇게 하면 안 된다.
        </div>
    );
}
```

- state 값을 임의로 변경하지 않는다!!
- 직접 변경하면 React가 Component를 다시 렌더링할 타이밍을 알아차리지 못한다.
- state 값을 변경하기 위해서 setState를 이용한다

### State를 변경하는 두 가지 방법

```jsx
// 1. setState 내에 변경할 값을 넣기
const [count, setCount] = useState(0);
setCount(count + 1);

// 2. setState에 함수를 넣기
const [count, setCount] = useState(0);

setCount((current) => {
  return current + 1;
});
```

- 변경할 값을 setState 안에 직접 넣는 방법이 있고, 함수를 넣는 방법이 있다.
- 현재 값을 기반으로 state를 변경하고자 할때 함수를 넣는 법을 권장한다.

### Object, Array를 갖는 State를 만들 때 주의사항

```jsx
// 1. setState 내에 변경할 값을 넣기
const [count, setCount] = useState(0);
setCount(count + 1);

// 2. setState에 함수를 넣기
const [count, setCount] = useState(0);

setCount((current) => {
  return current + 1;
});
```

- Object를 값으로 갖는 State도 만들 수 있다.
- 그러나 예시의 경우 React가 State의 변경을 감지하지 못한다.
- user object 안의 grade가 변경되었지만 user 자체는 변경되지 않았기 때문이다.

```jsx
const [user, setUser] = useState({ name: '민수', grade: 1 });

setUser((current) => {
  const newUser = { ...current };
  newUser.grade = 2;
  return newUser;
});
```

- 기존 user의 내용을 새로운 object에 담고 grade를 변경한다.

## 그렇다면 어디서 상태를 관리해야할까?

> 해당 상태에 의존적인 컴포넌트를 모두 포함하는 (최상위) 컴포넌트가 상태를 소유한다.
> 체크박스가 체크될때 바뀌는 object가 있다면 체크박스의 상태와 object가 만나는 곳에서 관리하면 된다.
> 상태와 상태를 바꿀 수 있는 callback을 같이 내려준다.

- [“Lifting State Up”](https://ko.reactjs.org/docs/lifting-state-up.html)
- [Sharing State Between Components](https://beta.reactjs.org/learn/sharing-state-between-components)

## 1급 객체(first-class object)란?

> 일급객체(First-class Object)란 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다.

### 일급객체의 조건

- 변수에 할당(assignment)할 수 있다.
- 다른 함수를 인자(argument)로 전달 받는다.
- 다른 함수의 결과로서 리턴될 수 있다.
- 위에 대한 조건으로 인해 알 수 있는 것은 함수를 데이터(string, number, boolean, array, object) 다루 듯이 다룰 수 있다는 점이다.

javascript에서 함수는 일급객체로 취급하여 함수 자체를 함수의 인자로 쓸 수 있고, 함수 객체를 return 할 수도 있다. component 분리시 자식 component에서 상위 setState에 접근 해야할때, javascript 함수가 1급 객체이기에 상위 component에서 props와 setState를 내려보낼 수 있다.

[출처](https://lakelouise.tistory.com/260)

## 결론

> 이번주는 state에 대해서 공부하고, 제대로 쓰는 방법이 있다는 것을 알게 되었다. 특히 1급 객체의 개념과 연결지어 생했을때 평소 다루지 않았던 방법으로 함수를 다뤄보는 부분들이 흥미로웠다.
> setState 안에서 직접 값을 넣어서 상태를 변경하기만 했었는데, 함수를 통해 값을 변경하는 방법도 알게 되었다.
> 3주차 예제를 통해 검색 필터링시에 텍스트와, 재고상태 두가지 조건을 함수의 인자로 객체로 넘겨주는 코드가 기억에 남는다.

```jsx
const filteredProducts = filterProducts(products, { filterText, inStockOnly });
```
