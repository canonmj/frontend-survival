# usehooks-ts

## usehooks-ts란?

[usehooks-ts](https://usehooks-ts.com) 는 typescript로 쓰인 react custom hook library 이다.
hook 별로 구현 코드, 예제가 다 있어서 적용하기 쉽고 custom hook 설계에도 도움을 얻을 수 있다.

## useBoolean

useToggle() 로 토글을 지정할 수 있고 `!isXXX`로 쓰는것보다 직관적이다. 아래 코드처럼 한눈에 알기쉽도록 함수를 반환하니 필요한 것만 뽑아서 쓰면 된다.

```js
return { value, setValue, setTrue, setFalse, toggle };
```

## useEffectOnce

마운트 시 한 번만 실행되는 useEffect의 수정된 버전이다. 큰 효용이 있다기보다는 빈 배열을 작성하거나 읽을 수고가 덜들고, 이름에서 역할을 명확히 할 수 있다.

## useFetch

네이티브 fetch()를 이용하여 데이터를 검색하는것을 목적으로하는 간단하고 기본적인 React Hook.

수신된 데이터는 useRef를 통해 애플리케이션에 저장(캐시)되지만 LocalStorage(useLocalStorage())를 사용하여 데이터를 유지할 수 있음.

컴포넌트가 마운트 될때나 url이 바뀔때 fetch가 실행된다.

기본적인 사용 사례를 위한 fetch hook이니 SSR을 고려하거나, 최적화 등 고급 사용법을 위해서는 Redux 같은 툴킷을 사용하라고 권장하고 있다.

```js
import { useFetch } from './useFetch'

const url = `http://jsonplaceholder.typicode.com/posts`

interface Post {
  userId: number
  id: number
  title: string
  body: string
}

export default function Component() {
  const { data, error } = useFetch<Post[]>(url)

  if (error) return <p>There is an error.</p>
  if (!data) return <p>Loading...</p>
  return <p>{data[0].title}</p>
}
```

## useInterval

setInterval을 함수 react component로 사용한 것이다. 가장 큰 차이는 arguments 가 동적이다는 것이다.

콜백 함수를 첫 번째 매개변수로 설정하고 두 번째 인수에 지연(밀리초)을 설정하세요. 지연 대신 null을 전달하면 타이머를 중지할 수 있다.

setInterval이 예상처럼 잘 동작하지 않고, 시간을 들여야하기 때문에 잘 활용할 수 있을 것 같다. custom hook을 쓰면 useInterval을 통해 더이상 안쓰는 setInterval에 대한 clear 작업을 해줄 수 있다.

```js
import { ChangeEvent, useState } from "react";

import { useInterval } from "usehooks-ts";

export default function Component() {
  // The counter
  const [count, setCount] = useState < number > 0;
  // Dynamic delay
  const [delay, setDelay] = useState < number > 1000;
  // ON/OFF
  const [isPlaying, setPlaying] = useState < boolean > false;

  useInterval(
    () => {
      // Your custom logic here
      setCount(count + 1);
    },
    // Delay in milliseconds or null to stop it
    isPlaying ? delay : null
  );

  const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    setDelay(Number(event.target.value));
  };

  return (
    <>
      <h1>{count}</h1>
      <button onClick={() => setPlaying(!isPlaying)}>
        {isPlaying ? "pause" : "play"}
      </button>
      <p>
        <label htmlFor="delay">Delay: </label>
        <input
          type="number"
          name="delay"
          onChange={handleChange}
          value={delay}
        />
      </p>
    </>
  );
}
```

## useEventListner

addEventListener의 option을 거의 같은 식으로 쓸 수 있다. 특히 dispatchEvent로 전달되는 커스텀 이벤트에 반응하기 좋다고 강력추천을 하셨다.

## useLocalStorage

페이지 새로 고침 후에도 로컬 저장소를 통해 상태를 유지할 수 있다. (다크모드 설정 등)
첫 번째 매개변수에 storage key를 전달하면 useState와 동일한 방식으로 사용할 수 있다.

이벤트를 통해(dispatchEvent + useEventListener) 다른 컴포넌트와 동기화하는 게 매우 중요한 특징이라고 한다.

```js
import { useLocalStorage } from "usehooks-ts";

// Usage
export default function Component() {
  const [isDarkTheme, setDarkTheme] = useLocalStorage("darkTheme", true);

  const toggleTheme = () => {
    setDarkTheme((prevValue: boolean) => !prevValue);
  };

  return (
    <button onClick={toggleTheme}>
      {`The current theme is ${isDarkTheme ? `dark` : `light`}`}
    </button>
  );
}
```

## useDakMode

사실 dark 모드에 대해서 제어하고 싶다면 useDarkMode를 사용하면 된다. isDarkMode, toggle, enable, disable 같은 세부 함수들이 있다다.

## swr

SWR은 캐시 무효 전략 stale-while-revalidate 에서 유래되었다. SWR은 먼저 캐시(stale)로부터 데이터를 반환한 후, fetch 요청(revalidate)을 하고, 최종적으로 최신화된 데이터를 가져오는 전략이다.(이제는 이런 전략 익숙하다)

조금 더 풀어서 말하면 데이터의 빠른 최신화를 위해 캐시와 fetch 데이터 간 비교(재검증)을 한뒤 다를 시 갱신을 하는 전략이다. SWR을 사용시 캐시 이슈 해결을 위해 데이터가 바뀌었는지 실시간으로 계속 확인을 한다.

[SWR](https://swr.vercel.app/ko)

```js
import useSWR from "swr";

function Profile() {
  const { data, error, isLoading } = useSWR("/api/user", fetcher);

  if (error) return <div>failed to load</div>;
  if (isLoading) return <div>loading...</div>;
  return <div>hello {data.name}!</div>;
}
```

> 이 예시에서, useSWR hook은 `key` 문자열과 `fetcher` 함수를 받는다. key는 데이터의 고유한 식별자이며(일반적으로 API URL) fetcher로 전달된다. fetcher는 데이터를 반환하는 어떠한 비동기 함수든지 가능하다. 네이티브 fetch 또는 Axios와 같은 도구를 사용할 수 있다.

hook은 세 개의 값을 반환합니다: 요청의 상태에 기반하여 `data`, `isLoading`, `error`.

## react-query

TanStack Query로 이름이 바뀐 react-query.
React App에서 데이터 관리/처리를 위한 라이브러리다. 사용시 React 애플리케이션에서 서버 상태를 가져오고, 캐싱하고, 동기화하고 업데이트하는 것을 매우 쉽게 만든다.

### 사용하는 이유

react-query를 사용함으로 서버, 클라이언트 데이터를 분리할 수 있고 다음의 앞서말한 장점들이 있다.

- 캐싱
- get을 한 데이터에 대해 update를 하면 자동으로 get을 다시 수행한다. (예를 들면 게시판의 글을 가져왔을 때 게시판의 글을 생성하면 게시판 글을 get하는 api를 자동으로 실행 )
- 데이터가 오래 되었다고 판단되면 다시 get (invalidateQueries)
- 동일 데이터 여러번 요청하면 한번만 요청한다. (옵션에 따라 중복 호출 허용 시간 조절 가능)
- 무한 스크롤 (Infinite Queries (opens new window))
- 비동기 과정을 선언적으로 관리할 수 있다.
- react hook과 사용하는 구조가 비슷하다.

### useQuery

> 데이터를 get 하기 위한 api이다. post, update는 useMutation을 사용한다.

첫번째 파라미터로 unique Key가 들어가고, 두번째 파라미터로 비동기 함수(api호출 함수)가 들어간다. (두번째 파라미터는 promise가 들어가야한다.)

return 값은 api의 성공, 실패여부, api return 값을 포함한 객체이다.

useQuery는 비동기로 작동한다. 즉, 한 컴포넌트에 여러개의 useQuery가 있다면 하나가 끝나고 다음 useQuery가 실행되는 것이 아닌 두개의 useQuery가 동시에 실행된다. 여러개의 비동기 query가 있다면 useQuery보다는 useQueries를 권장한다.

```js
import { QueryClient, QueryClientProvider, useQuery } from "react-query";

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}

function Example() {
  const { isLoading, error, data } = useQuery("repoData", () =>
    fetch("https://api.github.com/repos/tannerlinsley/react-query").then(
      (res) => res.json()
    )
  );

  if (isLoading) return "Loading...";

  if (error) return "An error has occurred: " + error.message;

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{" "}
      <strong>✨ {data.stargazers_count}</strong>{" "}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  );
}
```

### useMutation

값을 바꿀때 사용하는 api입니다. return 값은 useQuery와 동일하고 간단히 예시코드로도 충분히 설명을 대신할 수 있다.
아래는 간단한 로그인 예시이다.

```js
import { useState, useContext, useEffect } from "react";
import loginApi from "api";
import { useMutation } from "react-query";

const Index = () => {
  const [id, setId] = useState("");
  const [password, setPassword] = useState("");

  const loginMutation = useMutation(loginApi, {
    onMutate: (variable) => {
      console.log("onMutate", variable);
      // variable : {loginId: 'xxx', password; 'xxx'}
    },
    onError: (error, variable, context) => {
      // error
    },
    onSuccess: (data, variables, context) => {
      console.log("success", data, variables, context);
    },
    onSettled: () => {
      console.log("end");
    },
  });

  const handleSubmit = () => {
    loginMutation.mutate({ loginId: id, password });
  };

  return (
    <div>
      {loginMutation.isSuccess ? "success" : "pending"}
      {loginMutation.isError ? "error" : "pending"}
      <input type="text" value={id} onChange={(e) => setId(e.target.value)} />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={handleSubmit}>로그인</button>
    </div>
  );
};

export default Index;
```

React Query는 다른 상태 관리 라이브러리와 함께 사용할 수 있으며, 다른 라이브러리와 결합 사용이 가능하다.

[React Query 참고](https://kyounghwan01.github.io/blog/React/react-query/basic/#usemutation)
