## Node 

- Chrome V8 JavaScript Engine 으로 빌드된 JS 런타임 환경   
- JS를 브라우저 밖의 환경에서도 실행할 수 있도록 해주며, 우려되던 실행 속도 문제를 해결   

## NPM 

- Node Package Manager    

- Node.js에서 사용할 수 있는 모듈들을 패키지화하여 모아둔 저장소 역할 + 패키지 설치 및 관리를 위한 CLI (Command Line Interface) 제공  

  `npm init` : npm 프로젝트 생성 

  `npm init -y` : npm 프로젝트 생성 + package.json 생성 (별도의 설정 x)    

### Npm Dependencies 

- NPM 의존성  

- `devDependencies` 에는 개발시에만 사용하는 개발용 의존 패키지 기록  

  `ex>` ESLint   

  - ESLint는 코딩 스타일 가이드를 따르지 않거나 문제 있는 코드 등을 찾기 위해 사용되는 JS linter    

  - 코딩 컨벤션에 위배되는 코드, 안티 패턴 등을 자동 검출    

    `npm i -D eslint` / `npx eslint --init`  : 설치 및 설정  

    ```
    ※ 참고
    npm(Package Manager) - npx (Package Runner) 
    npx : npm의 5.2.0 버전부터 새로 추가된 도구로 npm과 비교대상이 아닌, npm을 좀 더 편하게 사용하기 위해 npm에서 제공해주는 하나의 도구  
    + npm 레지스트리에 올라가있는 패키지를 쉽게 설치 및 관리할 수 있게 도와주는 CLI 도구   
    -> 전역으로 관리되는 패키지, 로컬로 관리되는 패키지 등을 따로 업데이트하고 관리해줄 필요 없이, 일회성으로 원하는 최신 버전의 패키지를 npm 레지스트리에 접근해서 설치하고 실행 가능하도록 함
    
    npm@5.2.0 이상 버전만 깔려있으면 npx 명령어 사용 가능  
    기본적으로 설치되어있지만, npx 미설치 상태라면 아래 명령어로 설치 가능 
    
    npm install -g npx
    
    아래와 같은 상황에서 유용하게 사용 
    1. npm-run-script 를 사용하지 않고 로컬에 설치된 패키지 실행하는 경우 
    2. 일회성 명령으로 패키지 실행하는 경우 
    3. gist-based scripts를 실행하는 경우 
    4. 특정 node 버전의 script를 실행하는 경우 
    
    ```

    

- `npm install <name>` / `npm i <name>`  : npm 의존성 추가    

  `npm i --save-dev` / `npm i -D`  : 패키지 설치 + package.json 의 `devDependencies` 에 설치된 패지키와 버전 기록     



## Webpack 

- 최신 frontend framework에서 가장 많이 사용되는 모듈 번들러   

  - 모듈 번들러란? 

    - 웹 애플리케이션을 구성하는 자원(HTML, CSS, JS..) 을 모두 각각의 모듈로 보고, 이를 조합하여 병합된 하나의 결과물을 만드는 도구   

      :arrow_right:  각각의 모듈을 하나의 파일로 병합 및 압축하는 동작 = 모듈 번들링    

      :arrow_right:  빌드 == 번들링 == 변환     

  - 모듈? 

    - 프로그래밍 관점에서 특정 기능을 갖는 작은 코드 단위    
    - Webpack 에선, 웹 애플리케이션을 제작하는데 필요한 HTML, CSS, JS 등의 파일 하나하나를 의미   

- Webpakc의 필요 이유   

  - 파일 단위의 자바스크립트 모듈 관리   

    - JS의 변수 유효 범위는 기본적으로 전역 범위로 최대한 넓은 변수 범위     

      -> 어디서도 접근하기 편하다는 장점 존재   

      -> 하나의 html 파일에 동일한 변수명을 가진 변수가 포함된 2개의 js 파일을 같이 로딩하면, 의도치않은 중복 선언 및 재할당 등의 문제 발생 가능   

    - 즉, 파일 단위로 변수를 관리하고자 함   

      - 이전엔 AMD, Common.js 등의 라이브러리 활용   

  - 웹 개발 작업 자동화 도구  

    - frontend 개발 업무할 때, 코드를 수정하면 웹에서 새로고침하여 적용사항을 확인해야하는 번거로움 존재   
    - HTML, CSS, JS 압축 + 이미지 압축 + CSS 전처리기 변환 등의 작업이 반복됨   
    - 위와 같은 번거로움을 해결하기 위해 Grunt, Gulp 등의 자동화 도구 등장  

  - 웹 애플리케이션의 빠른 로딩 속도 + 높은 성능 

    - 웹 사이트의 로딩 속도를 높이기 위해 브라우저 > 서버 로 요청하는 파일 숫자 줄이는 등의 노력   

      -> 그러기 위해 웹 개발 작업 자동화 도구를 이용하여 파일 압축 + 병합 작업 진행    

      -> Webpack를 이용해 여러 개의 파일을 1개로 합치면 브라우저별 HTTP 요청 숫자 줄이기 가능   

    - 초기 페이지 로딩 속도 증가를 위해 나중에 필요한 자원은 나중에 요청하는 

      **레이지 로딩(Lazy Loading)** 등장     

      -> 기존엔 Require.js 등의 라이브러리를 쓰지 않으면 동적으로 원하는 순간에 모듈을 로딩하는 것이 불가능  

      -> 이젠 Webpack의 Code Splitting 기능을 활용하여 원하는 모듈을 원하는 타이밍에 로딩 가능   

    **Webpack은 기본적으로 필요한 자원은 그때 그때 요청하자는 철학 보유**      

### Webpack Dev Server

[공식 깃헙](https://github.com/webpack/webpack-dev-server)  

> Use webpack with a development server that provides live reloading. This should be used for development only. 

- Webpack Dev Server는 개발 환경에서 사용    

- 코드만 변경하고 저장하면 Webpack으로 빌드한 결과를 자동으로 새로고침하여 브라우저 반영   

  `npm i -D webpack webpack-cli webpack-dev-server` : 설치 

  `npx webpack-dev-server` : 실행  

  ```json
  // package.json
  
  // ...
  "scripts": {
      "start": "webpack-dev-server",
    },
  ```

  - package.json을 위와 같이 수정하면, `npm start` 를 통해 webpack-dev-server 실행 가능   

    -> 실행 후, `http://localhost:8080` 링크를 통해 빌드 결과 확인 가능  
    
###### 출처 : Codesoom React 강의    
