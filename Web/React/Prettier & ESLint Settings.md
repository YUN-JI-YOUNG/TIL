## Prettier & ESLint Setting (with VSCode)    
- auto formatting 가능    
  => 협업 시 코드 컨벤션 지키는 것이 훨씬 수월해짐    
  => 그에 따라 코드 일관성 확보 가능    

         
### 1. ESLint 설치    
```    
npm install eslint
```    

### 2. Prettier 설치    
```     
npm install prettier
```     

### 3. VSCode Extension 설치    
- Prettier - Code Formatter    
  - 만약 VSCode File > Preferences > Settings 에서 `formatter` 를 검색해서 나오는     
    `Editor : Default Formatter` 설정이 `Prettier` 로 설정되어 있지 않다면, 설정 변경 필요    
    
### 4. ESLint 설정        
- `.eslintrc.js` 파일 생성 후 아래 코드 작성     

  ```jsx
  // .eslintrc.js

  module.exports = {
    env: {
      browser: true,
      commonjs: true,
    },
    extends: [
      'eslint:recommended',
      'plugin:react/recommended',
      'plugin:@typescript-eslint/recommended',
      'plugin:prettier/recommended',
    ],
    parser: '@typescript-eslint/parser',
    parserOptions: {
      ecmaFeatures: {
        jsx: true,
      },
      ecmaVersion: 2018,
      sourceType: 'module',
    },
    plugins: ['react', '@typescript-eslint', 'prettier'],
    rules: {
      /** 함수의 명시적 타입 리턴을 명시적으로 써주지 않아도 되도록 하는 옵션. 이건 off 하는게 맞을까? */
      '@typescript-eslint/explicit-module-boundary-types': 'off',
      'react/prop-types': 'off',
      'react/react-in-jsx-scope': 'off',
      'no-unused-vars': 'off',
      'prettier/prettier': ['error', { endOfLine: 'auto' }],
    },
  };
  ```    
  
### 5. Prettier 설정      
- `.prettierrc.js` 파일 생성 후 아래 코드 작성     

  ```jsx
  // .prettierrc.js

  module.exports = {
    arrowParens: 'always',
    printWidth: 80,
    quoteProps: 'consistent',
    singleQuote: true,
    semi: true,
    trailingComma: 'all',
    tabWidth: 2,
    useTabs: false,     
    orderedImports: true,    
    bracketSpacing: true,    
    jsxBracketSameLine: false,    
  };
  ``` 
  
    
### 6. VSCode 사용자 설정 변경    
- `Ctrl + Shift + P` 로 JSON 파일로 된 설정 파일 열어서 아래 코드 추가          

  ```json   
  "editor.defaultFormatter": "esbenp.prettier-vscode",
      "[typescriptreact]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      "[typescript]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      "editor.formatOnSave": true,
   ```    

