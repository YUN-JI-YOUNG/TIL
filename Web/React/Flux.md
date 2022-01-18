## Flux   

[공식문서](https://haruair.github.io/flux/docs/overview.html)  

- 페이스북이 제시한 클라이언트 웹 애플리케이션 아키텍쳐  
- MVC와 달리 단방향 데이터 흐름을 활용해 뷰 컴포넌트를 구성하는 React를 보완하는 역할  
  - 이전까지의 프레임워크와 달리 패턴과 같은 모습이므로 새 코드를 작성할 필요 없이 바로 Flux를 이용해 사용 가능  



### 핵심 구성   

- **Action** 
  - 새로운 데이터를 포함하고 있는 간단한 객체   
  - type 프로퍼티로 구분 가능   



- **Dispatcher**    

  - 모든 데이터는 중앙 허브인 dispatcher를 통해 흐른다     

  - 본질적으로 store의 콜백을 등록하는데 사용하며, 각각의 store를 직접 등록하고 콜백 제공      

  - action을 store에 배분해주는 간단한 작동 방식     

  - store 간에 의존성을 특정적인 순서로 콜백을 실행하는 과정 관리   

    => 앱의 규모가 커질수록 필수적인 역할   

    - Store는 다른 store의 업데이트가 끝날때까지 선언적으로 기다릴 수 있다.    

      혹은, 끝나는 순서에 따라 스스로 갱신 가능     

  - store 간의 의존성 관리는 `waitFor()` 메소드 활용   

    - `waitFor()` 는 단일 인수만 받고, dispatcher에 등록된 인덱스를 배열로 받음  
      - dispatcher에 등록된 인덱스 == **dispatch token**  
      - dispatch token은 콜백을 dispatcher에 등록할 때 사용하는  `register()` 메소드에서 반환    
    - 더 자세한 내용은 [문서](https://reactjs.org/blog/2014/07/30/flux-actions-and-the-dispatcher.html) 참고   



- **Store**   

  - 앱의 데이터와 비즈니스 로직 보유    

  - 전통적인 MVC 모델과 비슷하지만, 많은 객체의 상태 관리 가능  

    - 단순히 ORM 스타일의 객체컬렉션 관리를 넘어, 앱 내의 개별적인 도메인에서 앱의 상태 관리   

      => 독립적이므로,  `setAsRead()` 같은 직접적인 setter 메소드가 없는 대신, **dispatcher** 에 등록한 콜백을 통해 데이터 수신   

      => 어떤 액션이라도 관련이 있다면 전달   

  - 본인을 dispatcher에 등록하고, action을 파라미터로 받는 콜백 제공   

    - 콜백의 내부에선 switch문을 사용한 action 타입을 활용하여 action 해석,  

      store 내부 메소드에 적절하게 연결될 수 있는 훅 제공        



- **View (React Component)**     

  - controller-view 라고 불리는, 이벤트를 중계할 수 있는 특별한 view가 변경 이벤트 수신

    - store에서 얻을 수 있는 glue 코드 제공  
    - 데이터를 위계대로 자식들에게 전달하도록 도움   

  - store에게 이벤트 수신 > store의 퍼블릭 getter 메소드를 통해 새로 필요한 데이터 요청  

    - 이 과정에서 `setState()` 나 `forceUpdate()` 메소드 호출   
    - 호출 과정에서 자체의 `render()` , 하위 모든 자식의 `render()` 실행    

  - 가끔 컴포넌트의 단순함 유지를 위해 위계 깊은 곳에 controlloer-views 를 추가로 넣기도 한다.  

    => 특정 데이터 도메인에 관계된 위계 영역을 감싸 독립적(encapsulate) 으로 만드는데 도움   

    => 단일 데이터 흐름과 상충해 잠재적으로 새로운 데이터 흐름의 시작점에서 충돌 할 수 있으므로 **주의 필요!! **   

    => 여러 데이터의 업데이트의 흐름이 위계와 다른 방향으로 흐르지 않도록 고려    

<br>    

### action creator 메소드

- dispatcher를 돕는 메소드(헬프 메소드)이며, 앱에서 가능한 모든 변화를 표현하는 유의적 API 지원에 사용됨   
- 주로 View에서의 상호작용에서 발생하는 action 한테서 제공받는 메소드  

<br>   

### 데이터 흐름 구조       

1. view 는 사용자의 상호작용에 응답하기 위해 새로운 action을 만들어 시스템에 전파  
2. action은 dispatcher에게 action creator 메소드를 제공   
3. dispatcher는 store를 등록하기 위한 콜백을 실행하고, action을 모든 store로 전달  
4. store는 action에 대한 응답으로 스스로 갱신 > 본인이 변경되었다고 모두에게 전달  
5. store는 change 이벤트를 controlloer-views에게 알려준다.  
6. controller-views는 이벤트를 듣고 있다가 이벤트 핸들러가 있는 store에서 데이터를 다시 가져온다   
7. controller-views는 스스로의 `setState()` 메소드 호출하고, 컴포넌트 트리에 속해 있는 자식 노드 모두를 다시 렌더링     



- MVC 패턴(Model - View - Controller) 과 혼동 금지! 

  Controller는 Flux 에도 존재하지만, 최상위에선 `controller-views` - views 관계로 존재   

  **Controller-views** : Store 에서 데이터를 가져와 자식에게 보내는 역할       



- React View 에서 사용자가 상호작용을 할 경우 =>

  **View** : 중앙의 dispatcher를 통해 action 전파  

  **Store** : action이 전파되면 action에 영향있는 모든 view 갱신    

  => 이러한 방식은 React의 선언형 프로그래밍 스타일 (= view가 어떤 방식으로 갱신되는지 모두 작성하지 않아도 데이터를 변경할 수 있는 형태)에서 편리     



- store을 이용하여 제어 뒤집기     

  - 일관성 유지를 명목으로 외부의 갱신에 의존하는 방식과 달리 store는 갱신을 받아들이고 적절하게 조화   

  - store 바깥에 아무것도 두지 않는 방식  

    => 데이터의 도메인 관리가 필요없어져 외부의 갱신에 따른 문제를 명확히 분리   

  - MVC 패턴에선 다루기 어려운 작업들이 좀 더 수월해질 수 있음 

    `ex>` 읽지 않은 메시지가 강조되어 있으면서도 읽지 않은 메시지 수를 상단 바에 표시  

  
