## JSX 장점 

- 보기 쉽고 익숙하다. 

- 오류 검사.
  오류가 있을 때, 바벨이 코드를 변환하는 과정에서 이를 감지해 낸다. ex) 태그를 닫지 않았을 때.

- 더욱 높은 활용도 
  HTML 태그도 사용할 수 있고 컴포넌트 안에서도 작성할 수 있다. 


## JSX 문법 

- 감싸인 요소 
 컴포넌트에 여러 요소가 있다면 부모 요소 하나로 꼭 감싸야 한다. Virtual DOM에서 컴포넌트 변화를 감지해 낼 때, 효율적으로 비교할 수 있도록 컴포넌트 내부는 DOM 트리 구조 하나여야 한다는 규칙이 있기 때문. 
  

### Fragment 
  div 같은 것으로 감싸지 않고 여러 요소를 렌더링하고 싶다면 리액트를 불러올 때, Component와 함께 Fragement를 불러와서 아래와 같이 사용하면 된다. 

```JS
import React, { Component, Fragment } from 'react';

class App extends Component {
    render() {
        return (
            <Fragment>
             <h1>hello, React!</h1>
             <h2>How are you?</h2>
             </Fragment>
        );
    }
}

export default App;
```

결과 -> <img width="271" alt="스크린샷 2020-02-27 오후 10 59 25" src="https://user-images.githubusercontent.com/49551135/75452021-d6be4300-59b4-11ea-902a-5283bd5533f1.png">



- if 문 대신 조건부 연산자 
 JSX 내부의 자바스크립트 표현식에서 if 문을 사용할 수는 없다. 조건에 따라 렌더딩 해야 할 때는 JSX 밖에서 if 문을 사용하여 작업하거나, { } 안에 조건부 (삼항) 연산자를 사용. 
```JS
.
.
.
render(){
    const text = 'How are you?';
    const condition = true;

    return (
        <div>
         <h1>Hello, React!</h1>
         <h2>{text}</h2>
         {
             condition ? '참' : '거짓'
         }
         </div>
    );
}

export default App;
```

- 인라인 스타일링 
 리액트에서 DOM 요소에 스타일을 적용할 때는 문자열 형태로 적용할 수 없다. 대신 CSS 스타일을 자바스크립트 객체 형식으로 만들어 적용해야 한다. 

 ```JS
.
.
.
render() {
  const text = 'How are you?';
  const condition = true;
  const style = {
    backgroundColor: 'gray',
    border: '1px solid black',
    .
    .
    .
  }
}

```

자바스크립트 객체 keydptjsms '-'dmf 사용할 수 없으므로, background-color는 backgroundColor로 바꾸어서 작성

- class 대신 className 
  리액트에서 class를 설정할 때는 class 키워드 대신 className으로 설정해야 한다. 