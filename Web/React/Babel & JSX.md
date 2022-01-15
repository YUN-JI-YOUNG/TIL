## JS 이용한 웹 개발 

- JavaScript 문법을 이용하여 웹페이지 개발   

  - `ex> document.getElementById , innerText, innerHTML, appendChild ...`  

- DOM 조작을 JS를 이용하여 추상화    

  ```js
  // index.js
  
  // 함수로 추상화
  function createElement(tagName, ...children) {
      const element = document.createElement(tagName);
      children.forEach((child) => {
          element.appendChild(child);
      });
      return element
  }
  
  // 추상화한 함수를 이용하여 DOM 조작
  
  document.getElementById('app').appendChild(
  	createElement(
      'div',
      createElement(
          'p',
          ...[1,2,3].map((i) => (
              document.createTextNode(`hello, world ${i} | `)
          )),
          document.createTextNode('hello, world'),
          document.createTextNode('hi!')
      ),
      createElement(
          'p',
          document.createTextNode('hi')
      ),        
      )
  )
  ```

  - `const element = document.getElementById('app');` 처럼 일일이 다 선택해서 조작하는 것보다 위처럼 추상화하는 것을 지향    

  - 위의 코드를 조금 더 보기 좋게, 단순하게 표현하기 위해 **DSL(Domain Specific Language)** 활용    

    - 지금은 JS를 활용하여 작성했지만, html 과 비슷하게 코드를 작성할 수 있다면 더 직관적이고 보기 좋을 것   

      -> babel이 이러한 변환을 도와준다. (React의 경우, JSX..)  

    - DSL 을 활용한, 즉, JSX를 활용한 코드는 아래의 JSX 부분에서 다룰 예정   





## 발생한 에러 

#### 1. html 요소가 2번 반복되어 출력되는 문제 

- id 선택자로 선택한 요소에 appendChild로 새 요소를 추가하면 해당 요소가 2번 출력되는 문제 
- **:pushpin: 해결방법** 
  - `index.html` 에서 `<script src="main.js"></script>` 부분을 주석처리하면 해결 
  - 내보내는 과정에서 2번 내보내지는 걸로 추측   



<hr> 


## Babel

[공식문서](https://babeljs.io/docs/en/)  

- frontend 개발에서 Babel은 필수적   

  - 하지만, 실무환경에선 Babel을 직접 사용하기보단 Webpack으로 통합해서 사용하는 것이 일반적   

    -> 로더 형태로 제공 ( **babel-loader** )   

- Babel은 JavaScript 컴파일러   

  - 현재 및 이전 브라우저 또는 환경에서 ECMAScript 2015+ 코드를 이전 버전의 JS로 변환하는데 주로 사용되는 도구 체인    

    -> 타입스크립트, JSX 처럼 다른 언어로 분류되는 것도 포함   

    -> 이렇게 변환된 파일을 **트랜스파일** 이라고 함   

    - 트랜스파일은 추상화 수준을 유지한 상태로 코드 변환   
    - 빌드는 변환 전후의 추상화 수준이 다름   

  - 최대한 합리적으로 ECMAScript 표준에 충실하려고 노력   

  - 소스 맵 지원으로 컴파일된 코드를 쉽게 디버깅 가능   

- 주요 기능 

  - 변환 구문 
  - `core-js` 같은 제3자 Polyfill 을 통해서, 대상 환경에서 누락된 Polyfill 기능
  - 소스코드 변환 (codemods)  
  - [더보기 영상](https://babeljs.io/videos.html)  

- Babel은 코드를 받아서 `파싱 > 변환 > 출력`으로 작업 진행    

  [더 자세한 내용](https://jeonghwan-kim.github.io/series/2019/12/22/frontend-dev-env-babel.html)  

  - Babel : `파싱` , `출력` 담당

  - **플러그인** : `변환` 작업 처리      

    - 플러그인 형식은 visitor 객체를 가진 함수 반환해야 함     

      visitor 객체는 babel이 파싱하여 만든 추상 구문 트리(AST)에 접근 가능한 메소드 제공   

      - visitor 객체가 제공하는 메소드 중, Identifier() 메소드의 동작원리를 살펴보는 예제 코드 

      ```js
      // myplugin.js
      
      module.exports = function myplugin() {
        return {
          visitor: {
            Identifier(path) {
              const name = path.node.name
      
              // babel이 만든 AST 노드를 출력한다
              console.log("Identifier() name:", name)
      
              // 변환작업: 코드 문자열을 역순으로 변환한다
              path.node.name = name.split("").reverse().join("")
            },
          },
        }
      }
      ```

      - `npx babel app.js --plugins ./myplugin.js` 명령어로 플러그인 추가하여 사용    

      - 만약 추가할 플러그인이 많다면, 바벨 설정파일 `babel.config.js` 추가하여 해당 파일에 작성할 수 있다. 

        ```js
        // babel.config.js 
        
        module.exports = {
          plugins: [
            "@babel/plugin-transform-block-scoping",
            "@babel/plugin-transform-arrow-functions",
            "@babel/plugin-transform-strict-mode",
            "./myplugin.js",
          ],
        }
        ```

  

  - **프리셋** 
    - 위와 같은 플러그인 여러 개를 모아둔 것   
    - ECMAScript+ 환경은 env 프리셋 사용  
  - **Polyfill** 
    - Babel이 변환하지 못한 코드를 Polyfill 이라 부르는 코드조각을 불러와 결과물에 로딩해서 해결   
  - **babel-loader** 
    - babel을 직접 사용하기 보단, babel-loader를 활용하여 Webpack과 함께 사용하면 훨씬 단순하고 자동화된 Frontend 개발환경 가능    



- `npm i -D babel-loader`     

  `npm i -D @babel/core` 

  `npm i -D @babel/preset-env @babel/preset-react`  

  -> 바벨 로더 / 바벨 / 프리셋 설치   

  ```js
  // webpack.config.js
  // Webpack 설정에 로더 추가 
  
  module.exports = {
    module: {
      ...,
      rules: [
        {
        	...
        // .js 확장자로 끝나는파일은 babel-loader가 처리하도록 설정  
          test: /\.jsx?$/,
        
        // 사용하는 third-party 라이브러리가 많을수록 babel-loader가 느려질 수도 있으므로 node_modules 폴더는 예외처리
          exclude: /node_modules/,
        
        // 바벨 로더 추가  
          loader: "babel-loader", 
        	...
        },
      ],
      ...
    },
  }
  ```

  

  ```js
  // babel.config.js
  // preset 설정  
  
  module.exports = {
    presets: [
      [
        '@babel/preset-env',
        {
          targets: {
            node: 'current',
          },
        },
      ],
      '@babel/preset-react',
    ],
  };
  ```



## JSX 

 [공식문서](https://ko.reactjs.org/docs/introducing-jsx.html) 

`const element = <h1>hello, world</h1>;`  

- JS를 확장한 문법으로, UI가 어떻게 생겨야 하는지 설명하기 위해 React와 함께 사용할 것을 권장  

  - React는 JSX 사용이 필수는 아니지만, [Declarative Programming](https://ko.wikipedia.org/wiki/선언형_프로그래밍) (선언형 프로그래밍)을 도와준다. 
  - 그래서 JS 코드 안에서 UI 관련 작업을 할 때, 시각적으로 더 도움이 되며, React가 더 도움이 되는 에러 및 경고 메세지 표시할 수 있도록 함    

- JSX는 React element 생성    

  - Babel은 JSX를 `React.createElement()` 호출로 컴파일  

- JSX의 중괄호 안에는 유효한 모든 JS 표현식 가능   

  - JSX도 표현식이므로, if 구문 및 for 반복문 안에서 사용하거나 / 변수에 할당하거나 / 인자로 받아들이거나 / 함수로부터 반환하는 등의 작업이 모두 가능  

- 기본적으로 React DOM은 JSX에 삽입된 모든 값을 렌더링하기 전에 이스케이프   

  -> 앱에 명시적으로 작성되지 않은 내용은 주입되지 X  

  -> 모든 항목은 렌더링되기 전 문자열로 변환 

  = 즉, XSS 공격 방지 가능  

- JSX 를 활용하여 웹 개발 코드 작성   

  ```js
  /* @jsx createElement */
  
  function createElement(tagName, props, ...children) {
      const element = document.createElement(tagName);
  
   // props 가 Object 형태로 들어오는데, entries는 키와 밸류값을 묶어서 뽑아준다 
      // {'1' : 2, 'class':hi} => [['1', 2], ['class', hi]]
      // props 가 null 인 경우를 대비하여 props or {} 로 받기
      Object.entries(props || {}).forEach(([key,value])=>{
          element[key.toLowerCase()] = value;
          // 모두 소문자로 들어가야 하므로 변환
      })
  
      children.flat().forEach((child) => {
          // child 가 text 타입이 아닌, Node 타입이면 그대로 삽입 가능
          if (child instanceof Node) {
              element.appendChild(child);
              return;
          }
          // element.appendchild(child) 에서  child는 node가 아닌 text 타입이므로 추가 불가하여 텍스트 노드로 변환
          element.appendChild(document.createTextNode(child));
      });
      return element
  
  }
  
  let count = 0;
  
  function handleClick() {
      count += 1
      render();
  };
  
  function handleClickNumber(value) {
      count = value;
      render();
  }
  
  function render(){
  const element = (
      <div id="hello" className="greeting">
          <p>Hello, world</p>
          <p>
  {/* 함수를 쓸 때, handleClick 혹은 () => handleClick() 2가지 방법 가능 */}
              <button type="button" onClick={handleClick}>
                  Click me 
                  ( 
                      {count} 
                  )
              </button>
          </p>
  {/* 아래 같은 경우는 children에 할당될때, array 안에 또 array가 있는 형태로 들어가기 때문에 위의 함수에서 flat 활용 필요 */}
          <p>
              {[1,2,3].map(i => (
                  <button type="button" 
               onClick={() => handleClickNumber(i)}>
                      {i}
                  </button>
              ))}
          </p>
      </div>
  );
  // 이렇게 지우지 않으면 버튼이 중복되어 계속 추가됨
  document.getElementById('app').textContent = '';
  document.getElementById('app').appendChild(
      element
  )
  }
  render();
  ```

  - 위의 코드에서 let 대신 const 상수를 쓰고자 한다면, render 의 인자로 count 를 넣어주면 된다.  
    - JS 에서 let 사용은 지양하므로 const 사용을 지향하는데, const 는 재할당 불가    

  ```js
  const count = 0;
  
  // 상태가 count 말고 또 있을 수 있으므로, object 형태로 받기 
  
  function render({count}) {
      function handleClick() {
          render( {count : count + 1})
      }
      function handleClickNumber(value) {
          render( {count : value} )
      }
      ...
      
  }
  
  render( {count : 0})  // count 초기값 설정 
  ```

  



