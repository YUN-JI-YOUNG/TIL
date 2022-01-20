## Redux 활용 

- Redux 를 적용하여 상태를 store 에서 관리하기 위해 `App.jsx` 변경  

  ```jsx
  import React from 'react';
  import { useDispatch, useSelector } from 'react-redux';
  import Page from './Page';
  import {
      updateTaskTitle,
      addTask,
      deleteTask,
  } from './actions';
  
  
  function selector(state){
      return{
          taskTitle : state.taskTitle,
          tasks : state.tasks,
      }
  }
  
  export default function App() {
   	// const {tasks, taskTitle} = state; 
  	// 위처럼 구조분해 할당으로 state에서 꺼내오는 대신, useSelector 활용  
      // selector은 위로 따로 뺄 수도 있고, 안에 넣어서 작성도 가능 
      const {taskTitle,tasks} = useSelector(selector);
  
      // const [state, setState] = useState(initialState);
      // state 조작을 setState가 아닌 useDispathch 활용
      const dispatch = useDispatch();
      
      
      function handleClickAddTask(){
          // 상태 조작 관련 코드는 action creator인 addTask로 넘김
          dispatch(addTask())
      }
  
      function handleChangeTitle(event){
          dispatch(updateTaskTitle(event.target.value))
      }
  
      function handleClickDeleteTask(id){
          dispatch(deleteTask(id));
      }
  
      return (
          <Page tasks={tasks} onClickAddTask={handleClickAddTask}
          taskTitle={taskTitle}
          onChangeTitle={handleChangeTitle}
          onClickDeleteTask={handleClickDeleteTask}
          />
      )
  }
  ```

  - state 의 값을 꺼내올 때, 구조분해 할당 대신, useSelector 활용    

    - 추후 테스트 코드 작성 시, `Provider` 로 감싸줘야한다는 에러 발생  

    - 해결 방법 1

      ```jsx
      // App.test.jsx
      // 실제로 Provider로 감싸주기 
      
      test('App', ()=> {
          ...
          const {getByText} = render(
          	(
              <Provider>
                 <App />
              </Provider>  
          	)
          )
      })
      ```

    - 해결 방법 2 

      ```jsx
      // App.test.jsx
      // 가짜로 만들어서 useSelector를 쓸수 있도록 함
      
      jest.mock('react-redux');
      
      test('App', ()=>{
          ...
      })
      ```

      - mock을 사용하기 위해선, `react-redux.js` 파일 생성 필요  
        - **mock** 
          - 가짜로 되는 것들  
          - 이전에 의존관계를 끊고 독립적으로 테스트할 수 있게 해주는 mocking 참고    

      ```js
      // react-redux.js
      
      export const useDispatch = jest.fn()
      
      export const useSelector = jest.fn()
      ```

      - `App.jsx` 에서 사용한 Hook 이 `useDispatch` 와 `useSelector` 이므로 이 2개를 jest에서 지원하는 fn() 메소드로 가짜로 생성    
      - 하지만 가짜로 만든 것이므로 실제 값은 없어서 설정 필요  

      ```jsx
      // App.test.jsx
      
      test('App', ()=>{
          useSelector.mockImplementation((selector)=> selector({
              tasks : [
                  {id:1, title: 'todo1'},
                  {id:2, title: 'todo2'},
              ]
          }));
      })
      ```

      - useSelector 조작하여 값을 설정해두면 test 통과   

        => 이렇게 값을 설정해두면 초기 상태인 `InitialState` 의 tasks 는 빈 배열로 초기화 가능    

  

  - state 조작을 `setState`가 아닌 `useDispatch` 활용   

  - `Page` 로 함수를 넘겨줄 때, 따로 분해해서 쓰는 것이 아니라 한번에 쓰는 것도 가능   

    ```jsx
    // 기존 작성 코드 -> 따로 handleChangeTitle 함수 정의 필요
    onChangeTitle={handleChangeTitle}
    
    // 한 번에 작성하는 코드 -> 따로 함수 정의 필요 없음 + 보기에 복잡할 수 있음 
    onChangeTitle={(event)=> dispatch(updateTaskTitle(event.target.value))}
    ```

  - 상태 조작 관련 코드는 `action creator`인 addTask 등 한테 넘김   

    - action creator 들은 따로 `actions.js` 파일로 분해 가능     

      => `App.jsx` 에서 import 필요      

      ```js
      // actions.js
      
      export function updateTaskTitle(taskTitle) {
          return {
              type: 'updateTaskTitle',
              payload: {
                  taskTitle,
              }
          }}
      
      export function addTask() {
          return {
              type: 'addTask',
          }}
      
      export function deleteTask(id){
          return{
              type:'deleteTask',
              payload: {
                  id,
              }
          }}
      ```

- **store.js**     

  ```js
  import {createStore} from 'redux';
  
  // Redux action 
  // - type (string)
  // - payload => object => {taskTitle}
  
  const initialState = {
      newId:100,
      tasks: [],
      taskTitle : '',
  };
  
  function reducer(state, action) {
      if (action.type === 'updateTaskTitle'){
          return {
              ...state,
              taskTitle : action.payload.taskTitle,
      }}
      
      if (action.type === 'addTask'){
          const {newId, taskTitle, tasks} = state;
          return {
          ...state,
          newId : newId + 1,
          taskTitle: '',
          tasks: [...tasks, {id:newId, title:taskTitle}]
      }}
  
      if (action.type === 'deleteTask'){
          const {tasks} = state;
          return{
          ...state,
          tasks : tasks.filter((task)=> task.id !== id),
      }}
  
      return initialState;
  } 
  
  const store = createStore(reducer);
  
  export default store;
  ```

  - payload 는 object 타입이므로 구조분해 할당 가능   

  - 각 action 들은 action creator 이름인 type 으로 구분 가능     

  - 아무런 action 이 안들어오면 초기 상태인 `initialState` 반환        

  - reducer 함수는 이전 상태(state) 와 상태 변화(action 객체)을 parameter로 받음   

    - reducer 함수를 따로 파일로 분리 가능   

      ```js
      // reducer.js 
      
      const initialState = {
          newId:100,
          tasks: [],
          taskTitle : '',
      };
      
      export default function reducer(state = initialState, action) {
          if (action.type === 'updateTaskTitle'){
              return {
                  ...state,
                  taskTitle : action.payload.taskTitle,
          }}
          
          if (action.type === 'addTask'){
              const {newId, taskTitle, tasks} = state;
              return {
              ...state,
              newId : newId + 1,
              taskTitle: '',
              tasks: [...tasks, {id:newId, title:taskTitle}]
          }}
      
          if (action.type === 'deleteTask'){
              const {tasks} = state;
              return{
              ...state,
              tasks : tasks.filter((task)=> task.id !== action.payload.id),
          }}
      
          return initialState;
      } 
      ```

      - reducer는 이전 상태값과 action을 받아오므로, action 의 payload 값을 사용하기 위해선 `action.payload.[변수명]` 처럼 받아와야 함       

    

    -  reducer 함수는 상태를 조작하는 중요한 동작을 수행하므로, `reducer.test.js` 로 각 action 마다 테스트 코드 작성 권장  

