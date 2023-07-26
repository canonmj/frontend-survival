# VDOM

> Virtual DOM은 실제 DOM과 같은 내용을 담고 있는 복사본이라 할 수 있다. 복사본은 실제 DOM이 아닌 JS 객체형태로 메모리 안에 저장되어 있다.

## Reconciliation(재조정) 과정은 무엇인가?

> 리액트는 메모리에 VDOM 이라는 가상의 돔 트리를 존재시키면서, 기존 VDOM과 변경사항이 생긴 VDOM을 비교하여 실제 DOM을 업데이트 할지 결정한다. 이러한 프로세스를 reconcilation이라고 한다.
