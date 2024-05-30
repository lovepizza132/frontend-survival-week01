# 개발환경 세팅하는 법

## 3번째 실패... 이번에는 성공해야지

## Node

- 대부분이 Node.js를 사용한다. 그러니 Node.js로 세팅을 해보자!

- 핵심은 트렌드가 바뀌기 때문에 변화하는 개발환경에서 `전체적인 흐름을 파악하고 유연하게 대처하는 능력을 키우자!`

- Node.js는 반드시 LTS 버전을 설치하자

- FNM(Fast Node Manager)이 뭐야?

    Node.js 버전 관리 도구(nvm이 느려서 만듦)

    cross-platform 지원

        nvm은 window를 지원하지 않음(nvm-windows를 사용)
        nvm은 bash script 이기 때문에 window 미지원

    nvm(bash) < fnm(rust로 만듦)

    volta와 같이 프로젝트 진입 시 자동으로 node version 변환 가능

    설치와 구성이 nvm보다 쉬움

- 노드버전은 package.json에서 egines 필드로 node와 pnpm 버전 명시

    ```zsh
    {
        "engines": {
            "node": "14",
            "pnpm": "7"
        },
    }
    ```

----------------------------------------------------------------

### 개발환경 구축

### 1. Node.js 세팅(npm 패키지 준비)

- `package.json` 생성

    ```zsh
    npm init
    ```

  - npm init -y 한번에 package.json 생성
  - name은 kebab-case, Lisp-case

### 2. `.gitignore` 파일 생성

node_modules를 통째로 커밋하는 것을 방지하기 위해 `.gitignore` 파일 생성\
[Node - gitignore](https://www.toptal.com/developers/gitignore/api/node)

### 3. TypeScript 설정

TypeScript는 Javascript의 Superset이다.\
자바스크립트 언어에 정적인 타입을 입혀 개발과정에서 실수를 줄일 수 있다.

- 타입스크립트 devDependencies에 추가하기

    ```zsh
    npm i -D typescript
    ```

  - package.json에서 devDependencies에 들어가고 개발환경에서만 사용되는 툴
  - dependencies는 프로그램에서 사용
  - 과거에는 npm install --save-dev

- `tsconfig.json`파일 생성

    ```zsh
    npx tsc --init
    ```

  - node_modules/.bin/tsc --init 명령어와 같다.
  - npx는 node_modules에 해당하는 패키지가 설치되어 있으면 찾아서 실행, 만약 설치되어 있지 않더라도 npm 패키지들을 캐시하는 곳에 다운로드 받아서 설치하지 않아도 사용할 수 있도록 해준다.

- `tsconfig.json`타입스크립트 설정파일을 수정을 통해 JSX문법 허용

    ```zsh
    "jsx": "react-jsx",
    ```

  - .tsx 파일 사용 가능
  - import React를 하지 않아도 사용 가능

### 4. ESLint(정적 분석기) 설정

JavaScript, TypeScript의 정적 분석도구

- 주요 기능
  - 코드스타일 검사: 들여쓰기, 따옴표, 세미클론 등 스타일 규칙 검사
  - 오류 검사 잘못된 변수 사용, 선언되지 않은 변수 등 오류 검출
  - 경고 및 권고사항: 최적화, 가독성 향상 등을 위한 경고와 권고사항 제공
- Rules Severities
  - "off" or "0" : 규칙 해제
  - "warn" or "1" : 규칙을 경고로 설정
  - "error" or "2" : 규칙을 에러로 설정, 테스트, 빌드, PR에서 에러 발생
- 아래 명령을 통해 eslint와 관련된 dependecies를 설치

    ```zsh
    npm i -D eslint@8.57.0 eslint-config-xo@0.44.0 eslint-config-xo-typescript@4.0.0 eslint-plugin-react@7.34.1
    ```

- `.eslintrc.js` 파일 추가(다르게 되니 일단 진행)

    ```zsh
    npx eslint --init
    ```

- `.eslint.js`파일 내용 수정
JSX 문법에서 lint를 제외해주도록하고, jest를 사용하도록 수정

    ```zsh
    module.exports = {
        env: {
        browser: true,
        es2021: true,
        jest: true,
        },
        extends: [
        'xo',
        'plugin:react/recommended',
        'plugin:react/jsx-runtime',
        ],
        parser: '@typescript-eslint/parser',
        parserOptions: {
        ecmaFeatures: {
            jsx: true,
        },
        ecmaVersion: 12,
        sourceType: 'module',
        },
        plugins: [
        'react',
        '@typescript-eslint',
        ],
        settings: {
        'import/resolver': {
            node: {
            extensions: ['.js', '.jsx', '.ts', '.tsx'],
            },
        },
        },
        rules: {
        indent: ['error', 2],
        'no-trailing-spaces': 'error',
        curly: 'error',
        'brace-style': 'error',
        'no-multi-spaces': 'error',
        'space-infix-ops': 'error',
        'space-unary-ops': 'error',
        'no-whitespace-before-property': 'error',
        'func-call-spacing': 'error',
        'space-before-blocks': 'error',
        'keyword-spacing': ['error', { before: true, after: true }],
        'comma-spacing': ['error', { before: false, after: true }],
        'comma-style': ['error', 'last'],
        'comma-dangle': ['error', 'always-multiline'],
        'space-in-parens': ['error', 'never'],
        'block-spacing': 'error',
        'array-bracket-spacing': ['error', 'never'],
        'object-curly-spacing': ['error', 'always'],
        'key-spacing': ['error', { mode: 'strict' }],
        'arrow-spacing': ['error', { before: true, after: true }],
        'import/no-extraneous-dependencies': ['error', {
            devDependencies: [
            '**/*.test.js',
            '**/*.test.jsx',
            '**/*.test.ts',
            '**/*.test.tsx',
            ],
        }],
        'import/extensions': ['error', 'ignorePackages', {
            js: 'never',
            jsx: 'never',
            ts: 'never',
            tsx: 'never',
        }],
        'react/jsx-filename-extension': [2, {
            extensions: ['.js', '.jsx', '.ts', '.tsx'],
        }],
        },
    };
    ```

- Root 경로에 .eslintignore 파일 생성 및 .gitignore과 동일한 내용으로 작성해준다.

### 5. react 설치

- React와 React rendering을 위한 react-dom 설치

    ```zsh
    npm i react react-dom
    ```

- 오래된 라이브러리일 경우에는 내부에 타입 선언이 없기 때문에 타입 파일도 같이 설치

    ```zsh
    npm i -D @types/react @types/react-dom
    ```

### 6. Jest 설치(테스팅 라이브러리)

- 프론트엔드 테스팅을 위한 라이브러리 Jest 설치

    ```zsh
    npm i -D jest @types/jest @swc/core @swc/jest jest-environment-jsdom @testing-library/react @testing-library/jest-dom
    ```

  - `jest.config.js` 설정

    ```zsh
    module.exports = {
        testEnvironment: 'jsdom',
        setupFilesAfterEnv: [
            '<rootDir>/jest-setup.js.ts',
        ],
        transform: {
            '^.+\\.(t|j)sx?$': ['@swc/jest', {
                jsc: {
                    parser: {
                        syntax: 'typescript',
                        jsx: true,
                        decorators: true,
                    },
                    transform: {
                        react: {
                            runtime: 'automatic',
                        },
                    },
                },
            }],
        },
        testPathIgnorePatterns: [
            '<rootDir>/node_modules/',
            '<rootDir>/dist/',
        ],
    };
    ```

- `jest-setup.js.ts`디렉토리 생성 후 파일에 추가

    ```zsh
    /* eslint-disable import/no-unresolved */
    import '@testing-library/jest-dom';
    /* eslint-enable import/no-extraneous-dependencies */
    ```

- `@testing-library/jest-dom` 6.0.0 버전 이후 변경내용
    `jest.config.js` 파일 속 `setupFilesAfterEnv`의 `@testing-library/jest-dom/extend-expect`를 `jest-setup.js`파일에 `import '@testing-library/jest-dom`을 추가

- node_modules > eslint의 모든 파일의 에러 찾기 및 수정

    ```zsh
    npx eslint . # 찾기
    npx eslint --fix . # 수정
    ```

### 7. Parcel 세팅

- Parcel 설치

    ```zsh
    npm i -D parcel
    ```

- package.json scripts 수정
parcel로 서버 실행, 배포, ESLint 체크 및 Typescript 체크, Jest 테스트 간편 명령어 실행을 목적으로 npm scrip 수정

    ```zsh
    "source": "index.html",
    "scripts": {
        "start": "parcel --port 8080",
        "build": "parcel build",
        "check": "tsc --noEmit",
        "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
        "test": "jest",
        "coverage": "jest --coverage --coverage-reporters html",
        "watch:test": "jest --watchAll"
    },
    ```

- package.json main 수정

    node는 package.json의 main 사용\
    Web Server이기에 "source": "index.html"

### 에러

- `npm run lint` 에러

    ```zsh
    1:1  error  Definition for rule 'import/no-extraneous-dependencies' was not found  import/no-extraneous-dependencies
    1:1  error  Definition for rule 'import/extensions' was not found                  import/extensions
    ```

  - `npm i -D eslint-plugin-import` 설치 후 .eslinttr.js 수정

    ```zsh
    module.exports = {
    plugins: ['import'], // Specify the ESLint plugin
    extends: [
        // Extend recommended configurations
        'eslint:recommended',
        'plugin:import/recommended',
    ],
    rules: {
        // Additional rules or overrides as needed
    },
    };
    ```
