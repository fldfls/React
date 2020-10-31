## 컴포넌트 리렌더링 최적화 
10장에서 만들었던 일정 관리 프로젝트를 최적화하는 과정

```
문제접 찾기 -> 개발자 도구로 성능 분석하기 -> 컴포넌트 업데이트 성능 최적화하기 
```
* 컴포넌트 리렌더링 최적화를 할 때는 불필요한 리렌더링을 발견하는 것이 중요 



불필요한 렌더링을 방지하여 리렌더링 성능을 향상.



크롬 개발자 도구 Performance 탭을 이용하여 리렌더링 확인 

![ex_style `...`](./11.png) 

#### shouldComponentUpdate
```JS
    shouldComponentUpdate(nextProps, nextState) {
        return this.props.done !== nextProps.done;
    }
```

다음과 같은 상황에서 shouldComponentUpdate를 구현해야 한다

1. 컴포넌트 배열이 렌더링 되는 리스트 컴포넌트일 때
2. 리스트 컴포넌트 내부에 있는 아이템 컴포넌트일 때
3. 하위 컴포넌트 개수가 많으며, 리렌더링 되지 말아야 할 상황에서도 리렌더링이 진행될 때

리스트를 렌더링 할 때는 언제나 shouldComponentUpdate를 구현해 놓는 것을 습관화 하면 좋다 