## React 프로젝트 세팅

1. 초기설정 모두 yes로 간단 생성 `npm init -y`  

   

2. package.json 에 설정된 의존성 패키지 설치 `npm install`

   

3. index.html 파일 생성

   ```html
   <!DOCTYPE html> 
   <html>
       <head>
           <meta charset="UTF-8">
           <title>Demo</title>
       <body>
   				<div id="app"></div>
           <p>
               Hello,world
           </p>
   				<script src="main.js></script>
       </body>
       </head>
   </html>
   ```

   - **추후 index.js 혹은 index.jsx 파일을 생성하여 코드를 작성한다면,**

     **안 지우면 에러 발생!**

     

4. webpack, webpack-cli, webpack-dev-server 설치

   `npm i -D webpack webpack-cli webpack-dev-server`

   

5. package.json 에 start 명령어 입력

   ```json
   {
       ...
       "scripts": {
           ...
           "start" : "webpack-dev-server"
       }
   }
   ```

   - 이후부턴 `npm start` 로 간단하게 서버 실행 가능

     

6. `webpack.config.js` 파일 생성

   💡 webpack.config.js 파일의 변경사항을 반영하려면 서버 재시작 필요

   - webpack의 mode를 development 로 설정하기 위함

     → mode를 production 혹은 development 중 하나로 설정하지 않으면 에러 발생

   ```jsx
   const path = require('path');
   module.exports = {
       mode: 'development',
       entry: './src/index.js',
       output: {
           filename : 'main.js',
           path:path.resolve(__dirname, 'dist'),
       },
       module:{
           
       },
   }
   ```

   - `mode : 'development'` 부분만 필수, 나머지는 선택사항

     

7. src 폴더 및 index.js 파일 생성

   `./src/index.js`  

   - 만약, 여기서부터 `index.jsx` 파일로 생성하고자 한다면, webpack 설정에 entry 추가

     ```jsx
     module.exports = {
         ...
         entry: {
     				main : path.resolve(__dirname, './src/index.jsx')
     		},
         ...
     }
     ```

   

8. html-webpack 플러그인 설치 `npm i html-webpack-plugin`

   → webpack 설정에 추가

   ```jsx
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   
   ...
   module.exports = {
       ...
       plugins: [new HtmlWebpackPlugin({ template: './index.html'})],
   },
   ```

   - template 에는 index.html 경로를 찾아 작성

     

9. eslint 설치는 선택사항

   - `npm i -D eslint` 설치 > `npx eslint`  실행

     

10. babel-loader 및 babel-core , preset 설치

    `npm i -D babel-loader @babel/core @babel/preset-env @babel/preset-react`

    - jsx 문법을 활용하기 위해 필요

      

11. webpack 설정에 바벨 로더 추가

    ```jsx
    module.exports = {
    ...
      module: {
        ...,
        rules: [
          {
          	...
          // .js나 .jsx 확장자로 끝나는파일은 babel-loader가 처리하도록 설정  
            test: /\\.jsx?$/,
          
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

    

12. preset 설정을 위해 `babel.config.js 파일` 생성

    ```jsx
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
    };ba
    ```

    

13. react 설치 `npm i react react-dom`
