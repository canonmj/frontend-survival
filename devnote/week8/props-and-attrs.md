# Props와 Attr

- props
- attrs

## 가변 스타일링\_1

Styled Components는 React 컴포넌트에 넘어온 props에 따라 다른 스타일을 적용하는 기능을 제공합니다. Tagged Template Literals을 사용하기 때문에 함수도 문자열 안에 포함시킬 수 있다는 점을 이용하는데요.

예를 들어, 버튼의 글자색과 배경색을 props따라 바뀌도록 위에서 작성한 예제 코드를 변경해보겠습니다. 자바스크립트의 `||` 연산자를 사용하여 props이 넘어오지 않은 경우, 기존에 정의한 기본 색상이 그대로 유지되도록 합니다.

```jsx
import React from "react";
import styled from "styled-components";

const StyledButton = styled.button`
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 1rem;
  line-height: 1.5;
  border: 1px solid lightgray;

  color: ${(props) => props.color || "gray"};
  background: ${(props) => props.background || "white"};
`;

function Button({ children, color, background }) {
  return (
    <StyledButton color={color} background={background} Î>
      {children}
    </StyledButton>
  );
```

## 가변 스타일링\_2

prop에 따라 바꾸고 싶은 CSS 속성이 위와 같이 하나가 아니라 여러 개일 경우가 있습니다. 이럴 경우, Styled Components에서 제공하는 `css` 함수를 사용해서 여러 개의 CSS 속성을 묶어서 정의할 수 있습니다.

이번에는 자바스크립트의 `&&` 연산자를 사용해서, `primary` prop이 존재하는 경우에만 `css`로 정의된 스타일이 적용되도록 하였습니다.

```jsx
import React from "react";
import styled, { css } from "styled-components";

const StyledButton = styled.button`
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 1rem;
  line-height: 1.5;
  border: 1px solid lightgray;

  ${(props) =>
    props.primary &&
    css`
      color: white;
      background: navy;
      border-color: navy;
    `}
`;

function Button({ children, ...props }) {
  return <StyledButton {...props}>{children}</StyledButton>;
}
```

참고로 넘겨야할 prop 값이 많아질 경우, 위와 같이 `...props` 구문을 사용해서 `children` 외에 모든 prop을 간편하게 전달할 수 있습니다.

```jsx
import Button from "./Button";
<Button primary>Primary Button</Button>;
```

## 어트리뷰트

[https://velog.io/@ayaan92/styled-components-.attrs에-대하여](https://velog.io/@ayaan92/styled-components-.attrs%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)
