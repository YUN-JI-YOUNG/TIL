# CSS 레이아웃

###### 웹 페이지에 포함되는 요소들을 어떻게 취합하고 어느 위치에 놓을 것인지 제어     

- display
- position
- float
- flexbox
- bootstrap grid system



### 1. Float

###### 한 요소가 정상 흐름에서 빠져나온 후, 텍스트 or 인라인 요소가 그 주위를 감싸 요소의 좌,우측을 따라 배치되어야 하는 것을 지정

- 본래 이미지를 한 쪽에 띄우고 텍스트를 둘러싸는 레이아웃을 위해 도입

  ex. 신문 템플릿 등

- 더 나아가 다른 요소들에도 적용해서 전체 레이아웃을 만드는데까지 발전

- 기존의 흐름을 깨기 때문에, 블록 요소는 float 아래로 들어가게 되고, 텍스트나 인라인 요소는 들어가지 않고 주변에 남음      

  -> 레이아웃이 망가지지 않게 하기 위해 float clear 필요!

  -> 주로 "clearfix" 라는 클래스이름 + 가상요소(의사요소) `::after` 사용

  ​	-> 가상요소는 선택자에 추가하는 키워드로 선택요소의 일부분에만 스타일 적용 가능     

  -> 부모 요소에 float된 것을 무시하겠다는 클래스 추가       

  -> `display` 속성에 `block`를 지정    

  -> `clear` 속성이 제일 중요! `both, left, right` 으로 무시할 방향 지정 가능    

  -> 주로 `content` 속성과 같이 사용하며 요소에 장식용 콘텐츠를 추가할 때 사용      

  -> 위 세가지 속성이 보편적이며 이외의 속성은 문서 참고    

- 열 레이아웃을 만들 때 사용하고 있었으나, flexbox와 grid 등의 더 좋은 기술 출현으로 원래의 이미지를 위한 역할로 돌아감     

- MDN에선 이미 float를 레거시 레이아웃 기술로 분류하고 있지만, 네이버의  nav bar 등은 여전히 사용중 -> 사용자에 따라 사용하는 경우도 많다.    



#### 속성    

- none : 기본값   
- left : 왼쪽으로 띄움    
- right : 오른쪽으로 띄움    

</br>    

### CSS Flexible Box 레이아웃

###### 주로 Flexbox로 부름     

- 이전까진 CSS 레이아웃을 작성할 수 있는 도구는 float 혹은 positioning 뿐이어서 제한적이고 한계점 존재    

  -> positioning은 수평정렬 등이 힘듦        

- Flexbox 인터페이스 내의 아이템간 **"공간배분"**과 **"정렬"** 기능 제공하기 위한 1차원 레이아웃 모델로 설계      

- 기본 설정 

  - item은 행으로 나열 / 메인축의 시작부터 정렬     
  - item은 교차축의 크기를 채우기 위해 늘어남     
  - flex-wrap 속성을 nowrap으로 지정 (강제로 한줄에 배치)    

#### 요소

- Flex Container     
  - 부모 요소     
  - 가장 기본적인 모델이며, Flex item들이 놓여있는 영역     
  - 생성하려면 `display `속성도 `flex`나 `inline-flex`로 지정    
  - Container 내의 Container 중첩 가능    
- Flex item     
  - 자식 요소      
  - 컨테이너의 컨텐츠     
  - Flex Container의 속성이 Flex item들의 배치 결정       

#### 축

- 메인 축(main axis)
  - 방향을 어떻게 설정하느냐에 따라 x,y축 결정         
  - 왼쪽부터 시작(기본값)         
- 교차 축(cross axis)     
  - 위쪽부터 시작(기본값)    



#### 속성

- 배치 방향 설정     

  - flex-direction : default = row     
    - flexbox는 단방향 레이아웃이므로 메인축 방향만 설정       
    - row(→), row-reverse(←), column(↓), column-reverse(↑)       

- 메인축 방향 정렬

  - **justify**-content : default = flex-start   

    - `content : 여러줄` 

    - 속성값   
      -  `center` : 가운데 정렬      
      -  `flex-end` : 오른쪽 정렬      
      -  `space-between` : 좌우 정렬, 양 끝을 끝에 고정한 후 요소들끼리의 간격 동일     
      -  `space-around` : 균등 좌우 정렬, 외부의 너비 * 2 = 내부 요소 너비       
      -  `space-evenly` : 균등 정렬, 모든 너비 동일    

  - justify-items / justify-self 등은 auto margin으로 설정가능하므로 불필요     

</br>     



- 교차축 방향 정렬

  - **align**-items : default = `stretch` (늘리기)     

    - 한 줄 정렬   

    `flex-start`,`flex-end`,`center` : 해당 지점 기준 상하 정렬           

    `baseline` :  컨텐츠크기가 다를 때 기준 -> 폰트 크기를 조절해도 밑 시작은 동일하게 맞출 때  등의 경우에서 사용            

  - align-self       

    - 개별 요소 이므로 자식 요소(각 아이템)를 선택하여 작성

    `flex-start`,`flex-end`,`center` , `baseline`, `stretch`, `auto`

  - align-content       

    - 여러 줄 정렬       

    `flex-start`,`flex-end`,`center` ,  `stretch` ,`space-between`, `space-around`

- 기타

  - flex-wrap : default=nowrap

    - 강제로 한줄에 배치할지 여부 결정     
    - 내부 아이템이 많아 테두리 바깥으로 넘칠 때,      

    속성값을 `wrap`으로 지정하면 아래로 / `wrap-reverse`로 지정하면 반대로 넘친 요소가 위로    

  </br>   

  > 자식 요소에 직접 작성    

  - flex-grow : default=0  

    - **자식 요소**에 직접 작성      

    - 메인 축에서 남은 공간을 각 항목에 분배 - **상대적 비율 X**

      `a한테 1, b한테 2를 설정하면 남은 공간의 1/3을 a, 2/3을 b한테 분배` 

  - order : default=0

    - **자식 요소**에 직접 작성    
    - -1 > 0 > 1 등의 순서.    
    - 즉, order 값이 작을수록 앞으로 정렬    

- shorthand    

  - flex-direction + flex-wrap     
    - flex-flow: column wrap;     

