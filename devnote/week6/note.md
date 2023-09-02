서로 관심사가 다르고 서로 기능들이 다르다
설계 관점에서 (레이어드 아키텍쳐 - 백엔드)

사용자에 가까운것(ui), 사용자에게 먼것
사람->스위치->모터->바퀴

what’s your name ?
input
process -> hello, [name] -> 메세지
greeting(name: string) {
return `Hello ${name}`
}

input -> process -> output
3단계로 코드를 잘 구분만 해도 이해하고 유지보수하는데 도움이 된다.

model => Process
view => Output
Controller => Input

Flux Architecture: MVC의 대안
action - dispatcher - store - view

action = 이벤트/메세지
dispatcher = 여러 store로 action을 전달. 메세지 브로커?
store = 받은 action에 따라 상태를 변경
view = store 상태

스토어에는 dispatcher 가 존재해서 action 을 받고 reducer를 통해서 state를 변경한다

action을 어떻게 표현하느냐에 따라 사용성에 차이가 있음. 하지만 상태를 UI에 반영하는 방법은 모두 react 통해서
input => action + dispatch
process => reducer
output => view

external store
원래 UI 가 가장 바깥쪽
리액트안에 존재하지 않는 스토어

count 예제
useState 없이 count ++ 하면 화면에 반영되지 않는데 콘솔에는 반영됨
forceUpdate, useReducer

useState 안에서 useReducer를통해 동작 알아봄
forceUpdate에 내부적으로 상태를 변경하지 않으면 동작하지 않음 (count + 1 해주는 이유)

useCallback

관심사의 분리 (역할의 분리) 분업같은 느낌임
ui는 잘 바뀌어도 비즈니스 로직은 잘 바뀌지 않음

TSyringe (주사기)
의존 관계 주의
props drilling
polyfill 지원안하는곳에서 지원해줌

src > stores > Store.ts
import { singleton } from “tsyringe”

@singleton()
export default class Store {
count = 0;
}

끌어다 쓸때는 ?

import { container } from ‘syringe’;
import Store from ‘../stores/Store’;

const store = container.resolve(Store)
