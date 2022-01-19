## Redux 

[Usage with React](https://redux.js.org/tutorials/fundamentals/part-5-ui-react)  

- App 컴포넌트에서 관리하던 상태관리를 Redux 로 분리    

  - React 에서 `화면에 그려주는 내용을 갖는 컴포넌트(Presentational Components)` + `상태를 관리하는 컴포넌트(Container Components)` 로 관심사 분리했던 내용 참고   

  => 이제 상태를 한 곳에서 (ex. App) 관리할 필요 없이 Redux 가 관리해주므로, `useSelector` , `useDispatch` Hook 만 잘 사용하면 어디서든 상태 접근 가능   

  = `App.jsx` 파일을 여러개의 `Container Component` 로 나눠서 적절한 크기의 앱으로 조절 가능   



- Flux 아키텍처의 구현체 => Flux 에 대해선 이전 [문서](https://github.com/yjydev/TIL/blob/main/Web/React/Flux.md#flux)에서 서술   

- **Redux 설치**   

  `npm i redux react-redux`   

  => redux는 react에 종속적이지 않으므로 연결하려면 react-redux 필요  

- Redux 3가지 원칙   

  - 전체 상태값을 하나의 객체(store) 에 저장   
  - 상태값은 불변 객체 (const)  
  - 상태값은 순수 함수(Reducer)에 의해서만 변경되어야 함          

- 상태값을 수정하는 유일한 방법     

  == Action 객체와 함께 dispatch 메소드 호출      

- **Action**  

  - 상태값은 오직 action 객체에 의해서만 변경되어야 함   
    - type : (string)   
      - 어떤 액션인지 구별  
    - payload(object) : {taskTitle}    
      - 변경할 상태값     

- **Reducer**  

  - Redux에서 기존 상태를 다른 상태로 변경하는 함수    

  - 구조    

    ```
    function reducer(state, action) {
    	...
    	return state;
    }
    ```

  - 이전 상태값과 액션 객체를 입력으로 받아 새로운 상태값을 만드는 **순수 함수**   

    - 순수 함수는 함수 외부 상태 변경 불가  + 같은 입력값은 항상 같은 값 반환   

- **Store**  

  - Redux의 상태값을 갖는 객체   
  - store의 dispatch 메소드로 action의 발생 시작   
  - action 발생 > middleware funtion 실행 > reducer 실행 > 상태값을 새 값으로 변경   

- **Provider**  

  - React로 작성된 component들을 Provider 안에 넣으면 하위 component들이 Provider를 통해 redux store에 접근 가능   

- **react-redux hook**    

  - Hook이 등장하면서 상당히 갈끔한 코드 작성 가능  

    - 이전엔 `mapDispatchToProps`, `mapStateToProps` , `connect` 함수 등을 이용해 코드 양 多  

  - `useDispatch`, `useSelector` 가 가장 중요한 Hook     

    [참고](https://pks2974.medium.com/redux-hook-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-3b92b4d75466)   
