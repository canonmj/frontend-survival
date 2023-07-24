# JSX

## React에서 JSX를 사용하는 목적

> JSX는 XML 같은 ECMAScript(Javascript 표준)의 문법 확장이다. 작성된 부분을 React createElement을 쓰는 JavaScript 코드로 변환한다. 중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript 코드와 1:1로 매칭된다.

## Syntactic sugar

> 읽는 사람 또는 작성하는 사람이 편하게 디자인 된 문법이라는 뜻을 갖고 있다. JSX문법은 원래 작성해야하는 코드의 길이를 크게 줄여준다.

다음의 JSX는

```jsx
<Button onClick={() => alert('YES')}>Click me</Button>
```

다음처럼 번역됩니다.

```jsx
React.createElement(Button, { onClick: () => alert('YES') }, 'Click me');
```

> React를 사용할 때 JSX는 필수가 아니다.

하지만 각 JSX 엘리먼트는 React.createElement(component, props, ...children)를 호출하기 위한 Syntactic sugar라고 할 수 있다.

## React.createElement

createElement는 리액트 앨리먼트를 생성할 수 있게한다.

### Example #1

JSX 코드

```jsx
<p>Hello, world!</p>
```

변환된 JS 코드

```jsx
React.createElement('p', null, 'Hello, world!');
```

### Example #2

JSX 코드

```jsx
<Greeting name='world' />
```

변환된 JS 코드

```jsx
React.createElement(Greeting, { name: 'world' });
```

### Example #3

JSX 코드

```jsx
<Button type='submit'>Send</Button>
```

변환된 JS 코드

### Example #4

JSX 코드

```jsx
<div className='test'>
  <p>Hello, world!</p>
  <Button type='submit'>Send</Button>
</div>
```

변환된 JS 코드

```jsx
React.createElement(
  'div',
  { className: 'test' },
  React.createElement('p', null, 'Hello, world!'),
  React.createElement(Button, { type: 'submit' }, 'Send')
);
```

### Example #5

JSX 코드

```jsx
<div>
  <p>Count: {count}!</p>
  <button type='button' onClick={() => setCount(count + 1)}>
    Increase
  </button>
</div>
```

변환된 JS 코드

```jsx
React.createElement(
  'div',
  null,
  React.createElement('p', null, 'Count: ', count, '!'),
  React.createElement(
    'button',
    { type: 'button', onClick: () => setCount(count + 1) },
    'Increase'
  )
);
```

## React Element

> React 앱의 가장 작은 단위이고 화면에 표시할 내용으로, 브라우저 DOM 엘리먼트와 달리 일반 객체다. React DOM은 React 엘리먼트와 일치하도록 DOM을 업데이트합니다.

```jsx
const element = <h1>Hello, world</h1>;
```

[엘리먼트 렌더링](https://ko.legacy.reactjs.org/docs/rendering-elements.html)

<!-- ## React StrictMode -->
