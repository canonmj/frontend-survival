# Styled Components

## 기본문법

> 먼저 styled-components 패키지에서 styled 함수를 임포트합니다.
> styled는 Styled Components의 근간이 되는 가장 중요한 녀석인데요.
> HTML 엘리먼트나 React 컴포넌트에 원하는 스타일을 적용하기 위해서 사용됩니다.

• HTML 엘리먼트를 스타일링 할 때는 모든 알려진 HTML 태그에 대해서 이미 속성이 정의되어 있기 때문에 해당 태그명의 속성에 접근합니다.

```jsx
import styled from "styled-components";

styled.button`
  // <button> HTML 엘리먼트에 대한 스타일 정의
`;
```

• React 컴포넌트를 스타일링 할 때는 해당 컴포넌트를 임포트 후 인자로 해당 컴포넌트를 넘기면 됩니다.

```jsx
import styled from "styled-components";
import Button from "./Button";

styled(Button)`
  // <Button> React 컴포넌트에 스타일 정의
`;
```

Styled Components를 이용해서 JavaScript 코드 안에 삽입된 CSS 코드는 글로벌 네임 스페이스를 사용하지 않습니다. 다시 말해, 각 JavaScript 파일마다 고유한 CSS 네임 스페이스를 부여해주기 때문에, 각 React 컴포넌트에 완전히 격리된 스타일을 적용할 수 있게 됩니다.

이것은 순수하게 CSS만을 사용했을 때는 누리기 어려웠던 대표적인 CSS in JS의 장점 중 하나 입니다.

## 고정 스타일링

Styled Components 문법을 이용해서 간단하게 React로 작성된 버튼 컴포넌트를 스타일링 해보겠습니다. 우선 `<button>` HTML 엘리먼트에 원하는 스타일을 적용한 후 `StyledButton` 변수에 저장합니다.

이렇게 `styled` 함수가 리턴하는 것은 위에서 설명드린 것 처럼 React 컴포넌트이기 때문에 JSX를 통해 자유롭게 사용할 수 있습니다.

```jsx
import React from "react";
import styled from "styled-components";

const StyledButton = styled.button`
  padding: 6px 12px;
  border-radius: 8px;
  font-size: 1rem;
  line-height: 1.5;
  border: 1px solid lightgray;
  color: gray;
  background: white;
`;

function Button({ children }) {
  return <StyledButton>{children}</StyledButton>;
}
```

자, 이제 스타일이 적용된 이 버튼 컴포넌트를 다른 React 컴포넌트에서 다음과 같이 사용할 수 있습니다.

```jsx
import Button from "./Button";
<Button>Default Button</Button>;
```
