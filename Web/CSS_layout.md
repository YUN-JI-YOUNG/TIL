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

</br>    

### Bootstrap

- 트위터에서 시작된 오픈 소스 프론트엔드 라이브러리     

- 웹 페이지에서 많이 쓰이는 요소 거의 전부를 내장     

- 별도 디자인 시간이 크게 줄어들고 여러 웹 브라우저를 지원하기 위한 크로스 브라우징(브라우저마다 UI가 다르므로 웹 표준 적용)을 자동으로 해결해줘서 불필요한 시간 낭비 줄일 수 있음     

- one souce multi use       

  - 동일 소스에 대해 웹이든 모바일이든 어떤 플랫폼이든간에 잘 적용해서 보여줄 수 있게 함     
  - 반응형(responsive) 웹 디자인         
    - 별도의 기술이름이 아닌 웹 디자인에 대한 접근 방식 중 하나      
    - 반응형 레이아웃 작성에 도움이 되는 사례들의 모음 등을 작성하는데 사용되는 용어        
  - 반응형 웹 디자인 사용한 경우 -> 에어비엔비, 넷플릭스 등      
    - 웹용, 모바일용을 따로 만든 경우 -> 네이버 등    

- 일반 html 문서와 스타일 자체가 다르므로 노멀라이즈 작업 필요       

  -> 각 브라우저마다 다른 초기 스타일(user agent stylesheet)을 갖고 있기 때문에 최대한 표준에 맞도록, 어떤 브라우저든지 동등하게 보여지려면 브라우저 기본 값을 없애야 함     

  -> CSS 리셋 or 노멀라이즈 작업 필요

  - 노멀라이즈 : W3C의 표준을 기준으로 차이가 있는 브라우저를 수정     
  - 리셋 : 브라우저의 기본 스타일이 전혀 필요없다는 방식으로 접근 -> 무조건 리셋하므로 너무나 많은 선택자가 엉키고 불필요한 오버라이드가 많이 발생하여 디버깅이 힘듦         

- 사용법    

  - 파일 가져오기
    - head 부분에 외부스타일소스로 "bootstrap.css"를 불러오면 사용 가능     
    - js는 `</body>` 바로 위에 `<script src="bootstrap.bundle.js"></script>` 작성      
  - CDN 사용     
    - 부트스트랩 -> DOCS -> Quick start > 각 css, js 주소 링크복사하여 html 문서에 붙여넣기   
      - 링크에서 bootstrap.`min`.css 의 min은 파일의 용량 최소화 (가독성을 위해 작성된 공백 제거 등)            
    - 결국 인터넷 상에 올려져 있는 파일을 가져오는 것        

- CDN     

  - Content Delivery(Distribution) Network       
  - 컨텐츠의 효율적인 전달을 위해 서버<->사용자의 거리를 줄여 컨텐츠 로드 지연 최소화    
  - 분산된 서버로 이루어져 있으며, 전세계 사용자들이 본인에게 가장 가까운 서버에서 가져올 수 있으므로 로딩 시간을 늦추지 않고 사용 가능
  - 외부 서버를 활용하여 본인 서버의 부하 감소 가능     
  - 전세계 인터넷 절반 이상이 CDN 방식 사용중       

- 클래스 선택자     

  - 라이브러리 내에 여러 클래스들이 이미 구현되어 있어 사용하기만 하면 됨    
  - `.mt-1`  -> margin-top : 0.25rem(4px)        
  - 1 rem = 16px 이며, 1rem 기준으로 계산하며 `m-5 : (3rem)`까지 정의         
  - `.mx-0` -> x축이므로 margin-right : 0 & margin-left : 0        
  - `.mx-auto` -> 수평 중앙 정렬      
  - `.py-0` -> padding-y축(top/bottom)      
  - 참고 : `s` -> start (left) , `e` -> end (right)       

  ​       

  - 많이 사용하는 색도 클래스로 구현되어 있음  
    - Bootstrap > Docs > Search (colors > colors)        
    - 백그라운 색 red : `.bg-danger`        
  - Flexbox도 클래스로 구현           
    - `class="d-flex"` 선언 == `display : flex`          

</br>      



### Bootstrap Grid System     

- flexbox로 제작되어 container, rows, column으로 배치 및 정렬       

- 대표적으로 구글 뉴스에서 확인 가능        

- 중요한 2가지! 

  - **12개의 column**       
    - 적당한 숫자들 중 가장 약수가 많아서 다양한 구조를 짤 수 있다.            
    - 부모에 `class="row"` 설정한 후 각각의 자식에게 `class="col"`  작성    
    - `col` 는 12개를 자동으로 개수만큼 나눠서 차지     
    - `col-3` `col-6` `col-3` 이면 각각 3칸, 6칸, 3칸, 차지              
    - 너비는 백분율로 설정되어 있어 각각 차지하는 칸수를 유지하며 부모 요소를 기준으로 크기는 유동적으로 조정     
    - grid layout에서 내용은 반드시 column 안에 있어야 하고, 오직 column만 row의 바로 하위 자식이 가능            
    - 중간에 `<div class="w-100"></div>`를 작성하면 2줄로 작성 가능         
    - 합이 12가 아니면 한쪽으로 몰리고, 12보다 초과하면 아래로 떨어짐        
  - **6개의 grid breakpoints**        
    - 다양한 디바이스에 적용하기 위해 정한 특정 px 조건에 대한 지점     
    - 부트스트랩은 em이나 rem을 사용하고, breakpoint는 px 사용     
      - viewport 너비가 px 단위이고 글꼴 크기에 따라 변하지 않으므로        
    - 모든 요소가 breakpoints 마다 바뀔 필요는 없고, 선택하여 사용     
      - `<576px` : xs  `>=576px` : sm  
      - `>=768px` : md  `>=992px` : lg  
      - `>=1200px` : xl   `>=1400px` : xxl    
      - xs는 ` col-1` 처럼 사이즈 작성 X
      - `col-sm-1 ` -> 576px 이상 768px미만이면 1칸 차지     
      - `com-md-3` -> 768px 이상 992px 미만이면 3칸 차지      
      - `.d-md-none` -> md일때 안보이게 변경    
      - `.d-md-block` -> md일때 display를 block으로 변경    
      - `.d-none d-sm-block` -> 계속 안보이다가 sm부터 보이기 시작    
      - `.d-lg-none d-xxl-block` -> 계속 보이다가 lg부터 안보이고 xxl부터 보임     

- gutters     

  - grid 시스템에서 반응적으로 공간을 확보하고 컨텐츠를 정렬하는데 사용하는 column 사이의 padding      

  - `g-0` 이면 서로 붙고 `gx-5` 이면 ↔ 방향으로 떨어짐      

    - `gx-5` 는 3rem이니까 가로길이에 3rem만큼 더한다음 12칸을 개수로 나누는 것      

      그리고 각 칸은 3rem을 반으로 나눠 padding right,left로 가짐    

- nesting        

  - row > col-* > row > col-** 방식으로 중첩 사용 가능         
    - 12개중 *칸만큼 나누고 -> 그 *칸를 다시 **칸만큼 나누고 -> ...
  - 내부에서 따로 정렬해야 할 때 사용    

- offset    

  - 지정한 column 공간을 무시하고 다음 공간부터 컨텐츠 적용     
  - `offset-4` 는 4칸 이후 시작한다는 의미     
  - breakpoints와 같이 사용 가능 > `offset-md-4` 는 md사이즈에서는 4칸 이후 시작     



