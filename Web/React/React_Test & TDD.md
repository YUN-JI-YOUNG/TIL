## 리액트 테스트 

#### Jest 

- 자바스크립트 테스팅 프레임워크     

- 특징    

  - 간단한 설정만으로 테스트 실행 가능 
  - 풍부한 matcher를 제공하여 별도의 모듈 없이 테스트를 더 풍부하게 표현 가능  
    - matcher : JS 정규식 클래스  
    - `ex>` 테스트 코드 `expect().toBe()` 에서의 **toBe**   
  - Coverage도 별도의 설치 없이 확인 가능 
  - Mocking 등을 지원하여 테스트를 더 쉽게 가능하게 해주는 프레임워크 

- `npm i -D jest @types/jest babel-jest` 

  - `jest`, `@types/jest` , `babel-jest` 설치    
    - **@types/jest**  : Jest의 타입 정의를 갖고 있는 모듈. 주로 TypeScript에서 사용하지만, 자동완성 기능을 위해 설치    
    - 현재, jsx 문법을 사용하고 있으므로 babel-jest 설치 필요    

- `[컴포넌트명].test.js` 

  - 테스트 파일 생성   
  - 만약, Counter 컴포넌트에 대한 테스트 파일을 작성한다면, `Counter.test.jsx`  

- `npx jest` 

  - 테스트 실행 

- `npx jest --watchAll`  

  - 프로젝트 내의 테스트 파일들을 모두 감시  

    -> 변경되면 테스트 자동 실행   

- `npx jest --watchAll --verbose`  

  - 어떤 테스트를 통과한건지 까지 알 수 있음   



#### Assertion (=단정문)

- 기대값과 실제 값의 일치 여부 확인   

- `console.assert` => 기대값이 아니면 콘솔에 출력되도록 하여 빠른 확인에 용이  

  => 그러므로 테스트에 단정문 사용  



#### Signature  

- 모든 연산은 연산의 이름, 매개변수, 반환값 명세  

  => 시그니처  

  add 함수에 대해선 , name(add), parameters(x,y), return(result)     



### TDD 

[TDD FAQ](https://github.com/ahastudio/til/blob/main/blog/2016/12-03-tdd-faq.md)

- 테스트 주도 개발은 테스트가 개발을 주도하는, 즉, 코딩의 방향을 이끌어가는 방법  

- 모든 테스트를 완전히 자동화 하고 결과까지 스스로 검사하도록 하는 것    

  - 테스트를 작성하기 가장 좋은 시점은  프로그래밍을 시작하기 전.     

- 단위 테스트 vs TDD 

  - 단위 테스트는 구현에 대한 테스트  
  - TDD는 자동화된 단위 테스트 코드를 먼저 작성하여 테스트가 개발을 이끌어나가도록 하는 방식   

- TDD를 위한 2가지 룰 

  - 자동화된 테스트에서 실패하지 않는 한 새로운 코드 작성 X   
  - 중복 제거    

- TDD Cycle    

  - **Red Green Refactoring**  

  1. 처음에는 통과하지 못할(RED) 테스트 작성 
  2. 이 테스트를 통과하게끔 (Green) 코드 작성 
  3. 결과 코드를 최대한 깔끔하게 Refactoring  :arrow_backward: **핵심!!**  

  4. 위의 과정을 짧은 주기로 반복  



#### React Testing Library  

- 사용자와 동일한 방식으로 DOM 쿼리를 사용할 수 있게 함   

  - 즉, 실제 사용자가 앱을 사용하는 방식으로 테스트하여 올바르게 동작하는지 테스트 가능 
  - 화면에 어떻게 보이는지에 대한 테스트를 위한 render 함수 등등..

- `npm i -D @testing-library/react @testing-library/jest-dom`   설치   

  - `@testing-library/jest-dom` : jest의 matcher들을 확장하여 테스트 의도를 더 명확하게 표현     

  - 테스트 코드를 작성한 파일에서 `import '@testing-library/jest-dom'` 등으로 import 해서 사용 가능   

    => But, 매번 import 하긴 너무 힘드므로 `jest.config.js` 파일 생성  

- **jest.config.js**  

  ```js
  module.exports = {
      setupFilesAfterEnv :[
          './jest.setup',
      ],
      testEnvironment: 'jsdom',
  }
  ```

  - `The error below my be caused by using the wrong test environment, ... Consider using the "jsdom" test environment` 에러 발생  

    => Jest의 기본 테스트 환경은 Node로 설정되어 있는데, Node엔 DOM 객체가 없으므로 react test library가 제대로 작동할 수 없어서 테스트 환경을 jsdom으로 변경

  - **jest.setup.js**  

    ```js
    import '@testing-library/jest-dom'
    ```

    - 앞으로 import 할 것들은 여기에 작성하면 테스트 코드마다 일일이 import 하지 않아도 ok   



- **fireEvent**  

  - 테스팅에서 DOM 이벤트를 편리하게 발생시켜주는 메소드   

    `ex>` click, change ...

- **Mocking**  

  - 일부 기능을 테스트할 때, 의존 관계를 끊고 독립적인 테스트 가능   
  - `jest.fn()` 으로 함수 mocking 가능   
    - Jest 에서 제공하는 다른 mocking 방법은 [공식문서](https://jestjs.io/docs/mock-function-api) 참고  



#### Test Code   

- 기본적인 테스트 코드  

```jsx
// To do App 의 Item.test.jsx

import React from 'react';
import {render, fireEvent} from '@testing-library/react';
import Item from './Item';

test('Item', ()=>{
    const task = {
        id:1,
        title:'todo',
    };
    
    // jest가 미리 준비한 function(=fn) 으로 진행
    const handleClick = jest.fn()
    
    // Item 컴포넌트는 task 와 onClickDelete 를 parameter로 받음
    const {container, getByText} = render((
    	<Item task={task} onClickDelete={handleClick} />
    ));
    
    expect(container).toHaveTextContent('todo');
    // done 이라는 글자를 가진 요소가 있는지 테스트 => done 버튼이 존재하므로 통과
    expect(container).toHaveTextContent('done');
    
    // handleClick 이 눌리지 않았다
    expect(handleClick).not.toBeCalled();
    
    // done 글자를 가진 요소를 클릭
    fireEvent.click(getByText('done'));
    
    // handleClick 이 눌렸다
    expect(handleClick).toBeCalled();
    
    // handleClick이 1로 눌렸다 == id가 1
    expect(handleClick).toBeCalledWith(1);
});
```

- 이것 외에도 다양한 테스트 형식 존재  

  - `describe - context - it `    

    - `context`는 jest가 지원하지 않아서 `jest-plugin-context` 설치 필요    

      `npm i -D jest-plugin-context`   

    - 플러그인 없을 땐 `describe - describe - it` 으로 했어야 했음   

    ```jsx
    // describe - it => describe('List') => it('renders tasks')
    // describe - context - it
    
    // with tasks
    // - List renders tasks ...
    // - List renders "delete" button to delete a task 
    // without tasks  
    // - List renders no task message
    
    describe('List', ()=>{  
        const handleClickDelete = jest.fn()
    
        function renderList(tasks){
            return render((
                <List
                tasks={tasks}
                onClickDelete={handleClickDelete}
             />
            ));      
        }
    
        context('with tasks', ()=>{
            const tasks = [
                {id:1, title: 'Task-1'},
                {id:1, title: 'Task-2'},
            ]
    
        it('renders tasks', ()=>{
            const { getByText } = renderList(tasks);
            expect(getByText(/Task-1/)).not.toBeNull();
        })
    
        it('renders "완료" button to delete a task', ()=>{
            const { getAllByText } = renderList(tasks);
    
            const buttons = getAllByText('완료');
            // 각 할일마다 완료가 있으므로 몇번째 버튼인지 표시해야함
            fireEvent.click(buttons[0]); 
            
            // 각 할일마다 완료가 있으므로 몇번째 완료인지 표시해야함 
            // fireEvent.click(getByText('완료')); 
    
            // 눌렸는지
            expect(handleClickDelete).toBeCalledWith(1);
        })
        });
    
        context('without tasks', ()=>{
            
            it('renders no task message', ()=>{
                const tasks = [];
                const { getByText } = renderList(tasks);
                expect(getByText(/할 일이 없어요/)).not.toBeNull();
            })
        })
    })
    ```

    

  - `given`  

    - 주어진 데이터에 대해 테스트   

