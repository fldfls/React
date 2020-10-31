리액트 프로젝트를 진행하다 보면 컴포넌트를 처음으로 렌더링할 때 어떤 작업을 처리해야 한다든지 컴포넌트를 업데이트 하기 전 후로 어떤 작업을 처리해야 할 수도 있고, 또 불필요한 업데이트를 방지해야 할 수도 있다. 이때, 컴포넌트의 라이프사이클 메서드를 사용

## 이해

라이프사이클 메서드의 종류 총 10가지. Will 접두사가 붙은 메서드는 어떤 작업을 작동하기 전에 실행되는 메서드고, Did 접두사가 붙은 메서드는 어떤 작업을 작동한 후에 실행되는 메서드이다. 
라이프사이클은 총 세 가지, 즉 마운트, 업데이트, 언마운트 카테고리로 나눈다. 


#### 마운트 
- DOM이 생성되고 웹 브라우저상에 나타나는 것을 마운트라고 한다. 


<마운트할 때 호출하는 메서드>

컴포넌트 만들기 -> constructor -> getDerivedStateFromProps -> render -> componentDidMount



- constructor : 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드 
- getDerivedStateFromProps : props에 있는 값을 state에 동기화하는 메서드 
- render : 우리가 준비한 UI를 렌더링하는 메서드
- componentDidMount : 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드



#### 업데이트 
- 컴포넌트를 업데이트 할 때는 다음 총 네 가지 경우. 

1. props가 바뀔 때 
2. state가 바뀔 때 
3. 부모 컴포넌트가 리렌더링 될 때 
4. this.forceUpdate로 강제로 렌더링을 트리거 할 때 



<업데이트 할 때 호출하는 메서드>

props 변경 -> getDericedStateFromProps -> shouldComponentUpdate -> (false를 반환하면 취소) -> render -> getSnapshotBeforeUpdate -> (웹 브라우저상의 DOM 변화) -> componentDidUpdate 

- getDericedStateFromProps : 이 메서드는 마운트 과정에서도 호출하며, props가 바뀌어서 업데이트 할 때도 호출합니다. 
- shouldComponentUpdate : 컴포넌트가 리렌더링을 해야할지 말아야 할지를 결정하는 메서드. 여기에서 false를 반환하면 아래 메서드들을 호출하지 않는다. 
- render : 컴포넌트를 리렌더링.
- getSnapshotBeforeUpdate : 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드.
- componentDidUpdate : 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드.


#### 언마운트 
마운트의 반대 과정, 컴포넌트를 DOM에 제거하는 것.


<언마운트 할 때 호출하는 메서드>
언마운트 하기 ->  componentWillnmount


- componentWillnmount : 컴포넌트가 웹 브라우저 상에서 사라지기 전에 호출하는 메서드 





## 살펴보기 


#### render() 함수 

  render() {...} 

이 메서드는 컴포넌트 모양새를 정의. 라이프사이클 메서드 중 유일한 필수 메서드이기도 한다. 이 메서드 안에서 this.props와 this.state에 접근할 수 있으며 리액트 요소를 반환. 
다만 이 안에서는 절대로 state를 변행해서는 안 되며, 웹 브라우저에 접근해서도 안 된다. DOM 정보를 가져오거나 변화를 줄 때는 componentDidMount에서 처리해야 한다. 




#### constructor 메서드 
 constructor(props) {...}

생성자 메서드로 컴포넌트를 만들 때 처음으로 실행된다. 이 메서드는 초기 state를 정할 수 있다. 





#### getDerivedStateFromProps 메서드 

리액트 v16.3 이후에 새로 만든 라이프사이클 메서드. props로 받아 온 값을 state에 동기화시키는 용도로 사용하며, 컴포넌트를 마운트하거나 props를 변경할 때 호출.
```JS 
    static getDerivedStateFromProps(nextProps, prevState) {
        if(nextProps.value !== prevState.value) { //조건에 따라 특정 값 동기화 
            return { value: nextProps.value };
        }
        return null; //state를 변경할 필요가 없다면 null를 반환 
    }
```






#### componentDidMount 메서드 
 componentDidMount() { ... }
이것은 컴포넌트를 만들고, 첫 렌더링을 다 마친 후 실행. 이 안에서 다른 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 처리하면 된다. 




#### shouldComponentUpdate 메서드 
 shouldComponentUpdate(nextProps, nextState) {...}
props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드. 이 메서드 안에서는 반드시 true 또는 false 값을 반환해야 한다. 이 안에서 현재 props와 state는 this.props와 this.state로 접근하고 새로 설정될 props 또는 state는 nextProps와 nextState로 접근할 수 있다. 





#### getSnapshotBeforeUpdate 메서드 
```JS
getSnapshotBeforeUpdate(prevProps, prevState){
    if(prevState.array !== this.state.array) {
        const { scrollTop, scrollHeight } = this.list
        return { scrollTop, scrollHeight };
    }
}
```

render 메서드를 호출한 후 DOM에 변화를 반영하기 바로 직전에 호출하는 메서드. 주로 업데이트 하기 직전의 값을 참고할 일이 있을 때 활용. 예) 스크롤바 위치 유지. 




#### componentDidUpdate 메서드 
 componentDidUpdate(prevProps, prevState, snapshot) { ... }

prevProps 또는 prevState를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근할 수 있다. 또 getSnapshotBeforeUpdate에서 반환한 값이 있다면 여기에서 snapshot 값을 전달받을 수 있다. 



#### componentWillUnmount 메서드 
 componentWillUnmount() {...}
  
컴포넌트를 DOM에서 제거할 때 실행. componentDidMount에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기에서 제거 작업을 해야 한다. 













## 함수형 컴포넌트 
 앞에서 만든 것처럼 컴포넌트를 만들 때마다 클래스를 만들고, 또 그 안에 render 메서드를 만드는 것과 달리 오로지 props를 전달받아 뷰를 렌더링하는 역할만 한다면 컴포넌트를 간단하게 선언할 수 있다. 





 #### 사용법 
다음과 같이 순수 함수만으로도 선언할 수 있다. 
```JS
import React from 'react';

function Hello(props) {
    return (
        <div>Hello {props.name}</div>
    )  
}

export default Hello; 
```



### 언제 함수형 컴포넌트를 사용하는가?
라이프 사이클, state 등 불필요한 기능을 제거한 상태이기 때문에 일반 클래스형 컴포넌트보다 메모리 소모량은 적다. 성능도 더 빠르다. 컴포넌트를 만들 때는 대부분 함수형으로 작성하여 state를 사용하는 컴포넌트 개수를 줄이는 방향으로 만들면 된다. 
 