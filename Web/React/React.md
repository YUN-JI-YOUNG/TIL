## React

> A JavaScript library for building user interfaces  

[공식 사이트](https://reactjs.org/)  

- UI들을 독립적이고 재사용할 수 있는 부분으로 나누고, 각 부분을 분리하여 개발할 수 있는 컴포넌트(JS 함수 또는 객체)로 만들때 도움을 주는 라이브러리   

  :arrow_right: state 세팅 

  :arrow_right: 이를 기반으로 화면에 어떻게 보여지기 원하는지 작성하여 하나의 컴포넌트로 구성 

  :arrow_right:  React 내장 Component 라이브러리의 기능 import 

  :arrow_right:  내장된 render() 메소드를 통해 ReactDOM 라이브러리한테 렌더링될 컴포넌트 전달 

  결과적으로, ReactDOM 라이브러리가 현재 DOM과 전달받은 컴포넌트를 비교하여 변경이 필요한 부분만 변화시켜 화면에 렌더링

- 핵심 개념  

  - 가상 DOM (Virtual DOM)  

    - Declarative 방식 (선언형 방식)   

      -> 데이터의 흐름 방향 및 요소들 간의 연관성 파악 용이해짐  

      -> 코드의 복잡성 :arrow_down_small: / 코드 퀄리티 :arrow_up_small: 

  - Component 컨셉  

  - 일방향적인 데이터 흐름(One way Data Flow)   

    - State > component > 가상 DOM > 실제 DOM과 비교 > 화면 렌더링   
      - state, component는 JSX 문법 기반  

#### ReactDOM 

- `react-dom` 은 React에서 DOM에 특화된 메서드를 사용할 수 있도록 API 제공   

  - `ex>` React 요소를 DOM으로 렌더링하는 render 함수 등등..

- JSX를 React.createElement가 컴파일할 수 있도록 React를 직접 사용하지 않아도 import 필요  

  ```react
  import React from 'react';
  import ReactDOM from 'react-dom';
  
  function App() {
      return (
      	<div>
          	<p>Hi! </p>
          </div>
      );
  }
  
  ReactDOM.render(
      <App />, 
      document.getElementById('app')
  )
  ```



#### Components & Props 

- 리액트 훅이 등장하며, 특수 케이스를 제외화곤 모든 컴포넌트를 함수 컴포넌트로 개발할 것을 권장  
- Component : 자바스크립트 함수  
  - 입력값 ,반환값 존재  
  - 입력값 => 리액트의 props  
  - 반환값 => 어떤 element 를 보여줄지 결정   



### React 를 이용한 웹 개발

- `npm i react react-dom` : 리액트 설치   

  ```jsx
  // index.jsx
  
  import React from 'react'   // 이제 react 객체를 얻어와서 사용 가능
  import ReactDOM from 'react-dom'  // react 객체를 그리기 위해 필요 
  
  const element = (
  	<div>
      	<p>Hello, world</p>
      </div>
  )
  
  ReactDOM.render(element, document.getElementById('app'));
  ```

  - React 를 이용하여 얻은 React Element 들은 직접 그릴 순 없고, react-dom 을 이용하여 그릴 수 있다   
  - `/* @jsx React.createElement */  ` 도 React에선 사용하지 않아도 ok    
    - 원래 DOM 객체를 직접 만들어 쓸 때는 필요했지만, React에선 가상의 DOM 위에 그리고, 그것을 바탕으로 ReactDOM이 실제로 그려주기 때문에 개발 단계에서 몰라도 ok     
  - element 를 콘솔창에 찍어보면, `{$$typeof: Symbol(react.element), type: 'div', key: null, ref: null, props: {…}, …}` 

- Component 활용 

  ```jsx
  function Button({children}) {
      return (
        <button type="button">
            {children}
        </button>
      );
  };
  
  function renderButtons() {
      return (
        <p>
        {[1,2,3].map((i)=> (
        	<Button>
          	{i}
          </Button>    
        ))}  
        </p>
      )
  }
  
  function App() {
      return (
      <div>
      	<p>Hello, world</p>
          <Buttons />
      </div>
      );
  }
  
  ReactDOM.render(<App />, document.getElementById('app'));
  ```

  - 여기서 Button 처럼 각각 쪼개둔 것을 component 라고 한다.  

    - `<Button>` 태그에서 받은 값은 Button 함수에서 props 객체로 받을 수 있다. 

      -> 구조분해 할당으로 `{children}` 처럼 children 값만 받아올 수 있다.   

  - return 값에선 `<p>` 태그든 `<div>` 태그든 무조건 뭔가로 감싸줘야 한다.   

  - 위의 코드처럼 여러개의 버튼이 존재하는 경우, 각 버튼마다 고유의 key 값 필요  

    `<Button key={i}></Button>` 처럼 key 할당 가능   

  - function 으로 정의했지만 해당 함수를 실행하는 것이 아니라 html 처럼 사용    

    == 선언한 것

    - React는 **선언형(Declarative) 프로그래밍 방식**      

      = 프로그래밍 패러다임 중 하나로, 정확하게 어떤 명령 or 단계들이 실행될지 묘사하는 것이 아니라 프로그램에서 원하는 것 목표만을 기술   

      = **goal 만 존재**, **how 는 기술하지 않음**    

      `ex. Query language(SQL), Regular expression, 설정 파일, 함수형 프로그래밍`   



#### React Hook 

- Hook 

  - 함수 컴포넌트에서 React state와 생명주기 기능을 연동할 수 있게 해주는 함수  
  - class 안에선 동작하지 않는 대신, class 없이 React를 사용할 수 있게 해주는 것  
    - 이미 짜놓은 컴포넌트를 재작성하는 것은 권장하지 않으며, 새로 작성하는 컴포넌트부터 Hook 을 이용하면 되는 것     

  - React는 useState 같은 내장 Hook 몇 가지를 제공하며, 컴포넌트 간 상태 관련 로직을 재사용하기 위해 Hook을 직접 작성할 수도 있음  



- **useState**  
  - React의 내장 Hook 이며, state 변수 선언 가능  
  - 클래스 컴포넌트의 this.state가 제공하는 기능과 동일   
    - 함수 컴포넌트의 state는 객체일 필요 없고, 숫자 타입 / 문자 타입을 가질 수 있음        
  - 일반 변수는 함수가 끝날때 사라지지만, state 변수는 React에 의해 유지됨  
  - useState() Hook의 인자 == state 의 초기 값    
  - useState() Hook의 반환값 == state 변수, 해당 변수 갱신 가능한 함수    
    - 그러므로, `const [count, setCount] = useState()` 라고 작성   



- JSX만 사용하여 코드를 짤 때는 render 함수를 정의하여 `render()` 를 무언가 상태가 바뀔때마다 실행하여 DOM을 다시 그림    

  - 만약, React를 사용할 때도 그렇게 한다면 React를 사용하는 의미가 X  

    `React를 사용한다는 것` == 가상의 DOM을 활용하여 DOM에 그리는 것(=무언가 처리하는 것) 은 신경쓰지 않고 `이렇게 그려질 것이다.` 만 선언하여 처리하고자 하는 것  

    => 그러므로, 상태가 변경될 때마다 알아서 새로 그려지도록 `useState` 활용    

  ```react
  import React, {useState} from 'react'
  import ReactDOM from 'react-dom'
  
  // 상태를 보여주는 선언적 컴포넌트 Counter
  function Counter() {
      // 상태를 정의하는 내용
      const [state, setState] = useState({
          count: 0,
      });
      const { count } = state;
      
      // 상태를 변화시키는 내용 
      function handleClick() {
          setState({
              count : count + 1,
          })
      }
      
      return (
      	<button type="button" onClick={handleClick}>
          	Click me
              ({count})
          </button>
      )
  }
  
  function App() {
      return (
      <div>
      ...
      <Counter />
      </div>
      );
  }
  ReactDOM.render(<App />, document.getElementById('app'));
  ```

  - Counter 컴포넌트 안에 갖힌 상태(여기선 count)를 state 로 관리   
    - 전역이나 다른 컴포넌트에서 접근 불가  
  - state의 갱신이 가능한 함수 setState 로 버튼을 클릭할 때마다 state 갱신  
  - 결과적으론, state가 갱신될 때마다 우리가 `return` 부분에 선언한대로 그려지는 것  
    - 갱신될때마다 render 함수를 실행한다거나 등은 하지 않음   



#### 관심사 분리 

- 프로그램을 구별된 부분으로 분리시키는 디자인 원칙   

- React를 사용하는 가장 큰 이유    

- 코드를 이해하고 보수하기 훨씬 더 쉬워진다는 장점 존재   

- **기존의 웹 개발 방식** : 마크업, 디자인, 로직을 분리하는 기술의 분리  

  **React의 웹 개발 방식** : 컴포넌트별로 관심사 분리 (재사용성, 확장성 :arrow_up_small: )     

- 위의 Counter 컴포넌트에선, 상태를 정의하는 내용과 상태를 변화시키는 내용이 상태를 보여주는 선언전 컴포넌트 Counter 내부에 전부 다 들어가 있어서 무거움  

  => 가볍게 만들기 위해선, 화면에 그려주는 내용을 갖는 컴포넌트 + 상태를 관리하는 컴포넌트로 분리할 필요성 존재    

  => UI와 비즈니스 로직의 분리와 동일한 의미    

  화면에 그려주는 내용을 갖는 컴포넌트(UI) + 상태를 관리하는 컴포넌트(비즈니스 로직)    

  ```react
  // 화면에 그려주는 내용을 갖는 컴포넌트 Counter, Page
  
  function Counter({count, onClick2}) {
      return (
      	<button type="button" onClick={onClick2}>
          	Click me
              ({count})
          </button>
      )
  }
  
  function Page({count, onClick2}) {
      return (
      	<div>
          	<p>Hello, world</p>
              <Counter 
              count={count}
              onClick={onClick2}
              />
          </div>
      )
  };
  
  // 상태를 관리하는 컴포넌트 App
  
  function App(){
      const [state, setState] = useState({
          count: 0,
      });
      const { count } = state;
      
      function handleClick() {
          setState({
              count : count + 1,
          })
      }
      return (
      	<Page
          count={count}
          onClick={handleClick}
      	/>
      )
  }
  ```

  - 만약 컴포넌트별로 파일로 분리한다고 한다면, 

    1. Counter.jsx 파일 생성  

       ```jsx
       // Counter.jsx
       
       import React from 'react'
       
       function Counter({count, onClick}){
           return(
           ...
           )
       }
       
       export default Counter
       ```

       ```jsx
       // export default function Counter() 로 한번에 쓰는 것도 가능 
       
       export default function Counter({count, onClick}){
           return(
           ...
           )
       }
       ```

    

    2. index.jsx 에서 Counter 컴포넌트 import 

       `import Counter from './Counter'`   

    - 다만, `./Counter.jsx` 가 아니면 에러가 발생하는데, 확장자를 쓰고싶지 않다면 webpack.config.js 파일 수정 필요   

      ```js
      // webpack.config.js
      
      ...
      module.exports = {
          ...
          resolve:{
              extensions: ['.js', '.jsx'],
          },
          ...
      }
      ```

      이렇게 설정하고 나면, 이제 확장자 없이 Counter 로만 경로 입력해도 정상 작동   

    
