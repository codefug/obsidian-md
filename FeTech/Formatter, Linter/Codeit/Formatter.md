## 자동화 도구들

## Husky

커밋 전에 포매터와 린터를 자동으로 실행

Git에서 제공하는 git hooks라는 기능을 쉽게 사용할 수 있게 해주는 패키지일 뿐이다.

## Github Action

자동화 스크립트를 원격 저장소에서도 실행하는 프로그램

## Editor Config

Editor들의 설정을 공통적으로 적용하는 방법

https://editorconfig.org/

`.editorconfig` file를 조작하여 프로젝트 최상위 폴더에 배치하면 기본적인 에디터 설정 가능

```editorconfig
root = true

[*]
indent_style = space          # 들여쓰기에 스페이스 사용
indent_size = 2               # 들여쓰기 크기는 스페이스 2개로 쓰기
charset = utf-8               # 파일 인코딩은 utf-8
insert_final_newline = true   # 파일 맨 마지막에 빈 줄 추가하기
```

## Prettier

## 설치

```terminal
npm install --save-dev prettier
```

## 설정

[[https://prettier.io/docs/en/options.html|prettier options]]

```json
{
  "name": "formatter-and-lint",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "prettier": "prettier --write src/**/*.js" 
    // --write : 파일을 덮어써서 수정하겠다.
    // src/**/*.js  src directory 아래에 있는 모든 js 파일
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "prettier": "^2.8.8"
  }
}

```

이후 npm run prettier하면 prettier가 모든 파일에 규칙을 입힘

## Extension

vscode에서 prettier 설치 > ctrl + p > format with ~ > prettier 선택

setting > format on Save

## CI와 통합

Prettier같은 자동 포맷팅 도구는 CI과정의 일부로 통합되기도 함. 원격 저장소에 merge 될 때 포맷팅 처리

## 포맷팅 예외

.prettierignore 에서 `node_modules,package-lock.json` 같은거 넣어서 포맷팅 예외처리할 수 있음.

## ESLint

[[https://nextjs.org/docs/pages/building-your-application/configuring/eslint|Next js]]

## rules

rules에서 말그대로 규칙을 설정할 수 있다.

## extends

extends로 외부에 있는 규칙들을 가져와서 적용할 수 있다.

[[https://www.npmjs.com/package/eslint-config-airbnb|eslint-config-airbnb]]

```terminal
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    **'airbnb',
    'airbnb/hooks',**
  ],
  overrides: [],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: ['react'],
  rules: {},
};
```

## Prettier - ESLint

```terminal
npm install --save-dev eslint-config-prettier
```

```eslintrc

module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'airbnb',
    'airbnb/hooks',
    **'prettier',**
  ],
  overrides: [],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: ['react'],
  rules: {},
};

```

## ESLint 상세 옵션

https://blog.pumpkin-raccoon.com/74

## TS와 ESLint

https://typescript-eslint.io/

## StyledComponents와 ESLint

https://styled-components.com/docs/tooling#stylelint

## Husky

[[https://www.npmjs.com/package/husky|Husky]]

```terminal
npm pkg set scripts.prepare="husky install"
npm run prepare
npx husky add .husky/pre-commit "npm run lint"
// pre-commit git hook (commit 전) 에 npm run lint 실행
```

git hook으로 테스트 및 린트 자동화
https://blog.pumpkin-raccoon.com/85

