# LESSON-REACT-VITE

React さわったことないよーって人向けの React ＋ Vite で ToDo アプリ作って学ぶやつ

- [LESSON-REACT-VITE](#lesson-react-vite)
  - [ゴール](#ゴール)
  - [必要な準備](#必要な準備)
  - [Lessons](#lessons)
  - [Lesson01 環境構築しよう](#lesson01-環境構築しよう)
    - [Vite と React をインストール](#vite-と-react-をインストール)
    - [ESlint をインストール](#eslint-をインストール)
    - [prettier のインストール](#prettier-のインストール)
  - [まとめ](#まとめ)

## ゴール

- ローカルで動く簡単な TODO アプリを NestJS + React.js で作ってみる

## 必要な準備

とりあえず環境構築をして`node -v`,`npm -v`でバージョンが表示されれば OK

Ubuntu(WSL2)

```sh
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 21.04
Release:        21.04
Codename:       hirsute
```

Node

```sh
node -v
v16.14.2
npm -v
8.5.0
```

## Lessons

だいたいこんな感じ

- Lesson01 環境構築しよう　 ← いまここ
-

## Lesson01 環境構築しよう

### Vite と React をインストール

```sh
npm create vite@latest lesson-react-vite --template react-ts
```

いくつか質問されるのでこう答えます。

`React`を選択

```sh
? Select a framework: › - Use arrow-keys. Return to submit.
    Vanilla
    Vue
❯   React
    Preact
    Lit
    Svelte
    Others
```

`TypeScript + SWC`を選択

```sh
? Select a variant: › - Use arrow-keys. Return to submit.
    JavaScript
    TypeScript
    JavaScript + SWC
❯   TypeScript + SWC
```

インストールが終わったらディレクトリを移動して node_modeules をインストールする。

```sh
cd lesson-react-vite
npm install
```

インストールが完了したら起動できるか確認する。

```sh
npm run dev
```

こんな感じのメッセージと画面が出てくれば OK

```sh
VITE v4.2.1  ready in 233 ms

➜  Local:   http://localhost:5173/
➜  Network: use --host to expose
➜  press h to show help
```

### ESlint をインストール

使っていないプロジェクトはないのでインストールしちゃいます。

```sh
npm i -D eslint
```

以下のコマンドで対話式に設定をしてインストールします。

```sh
npx eslint --init
```

またいくつか聞かれるので答えていきます。

コードとスタイルをチェックする。

```sh
? How would you like to use ESLint? …
  To check syntax only
  To check syntax and find problems
▸ To check syntax, find problems, and enforce code style
```

JavaScript を選択

```sh
? What type of modules does your project use? (Use arrow keys)
▸ JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these
```

React を選択

```sh
? Which framework does your project use? …
▸ React
  Vue.js
  None of these
```

Yes を選択

```sh
? Does your project use TypeScript?  No / ‣ Yes
```

Browser を選択

```sh
? Where does your code run? …  (Press <space> to select, <a> to toggle all, <i> to invert selection)
▸ Browser
  Node
```

popular style guide を選択

```sh
? How would you like to define a style for your project? …
▸ Use a popular style guide
  Answer questions about your style
```

あとで AriBnb の設定をインストールするのですが一旦ここは Standard を選択

```sh
? Which style guide do you want to follow? …
▸ Standard: https://github.com/standard/eslint-config-standard-with-typescript
  XO: https://github.com/xojs/eslint-config-xo-typescript
```

JavaScript を選択

```sh
? What format do you want your config file to be in? …
▸ JavaScript
  YAML
  JSON
```

Yes を選択

```sh
eslint-plugin-react@latest eslint-config-standard-with-typescript@latest @typescript-eslint/eslint-plugin@^5.43.0 eslint@^8.0.1 eslint-plugin-import@^2.25.2 eslint-plugin-n@^15.0.0 eslint-plugin-promise@^6.0.0 typescript@*
? Would you like to install them now?  No / ‣ Yes
```

今回は npm を選択

```sh
? Which package manager do you want to use? …
▸ npm
  yarn
  pnpm

```

インストール完了後プロジェクト直下に`.eslintrc.cjs`ができていれば OK

続いて AriBnb の設定をインストールします。

```sh
npm install --save-dev eslint-config-airbnb eslint-config-airbnb-typescript
```

`.eslintrc.cjs`を以下のように編集します。

```js
module.exports = {
  root: true,
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  extends: [
    'plugin:react/recommended',
    'plugin:jsx-a11y/strict',
    'plugin:react-hooks/recommended',
    'plugin:storybook/recommended',
    //'standard-with-typescript',
    'prettier',
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: './tsconfig.json',
  },
  plugins: ['react', 'react-hooks', '@typescript-eslint', 'testing-library', 'jest-dom', 'import'],
  rules: {
    '@typescript-eslint/consistent-type-imports': ['error', { prefer: 'type-imports' }],
    'react/require-default-props': 'off',
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    'react/no-unknown-property': [
      'error',
      {
        ignore: ['css'],
      },
    ],
    'react/react-in-jsx-scope': 'off',
    'react/function-component-definition': [
      'error',
      {
        namedComponents: 'arrow-function',
        unnamedComponents: 'arrow-function', // 'function-declaration' | 'function-expression' | 'arrow-function'
      },
    ],
    'import/no-extraneous-dependencies': 'off',
    'import/order': [
      'error',
      {
        groups: ['builtin', 'external', 'parent', 'sibling', 'index', 'object', 'type'],
        pathGroups: [
          {
            pattern: '{react,react-dom/**,react-router-dom}',
            group: 'builtin',
            position: 'before',
          },
          {
            pattern: '@src/**',
            group: 'parent',
            position: 'before',
          },
        ],
        pathGroupsExcludedImportTypes: ['builtin'],
        alphabetize: {
          order: 'asc',
        },
        'newlines-between': 'always',
      },
    ],
  },
  overrides: [
    {
      files: ['**/tests/**/*.[jt]s?(x)', '**/?(*.)+(spec|test).[jt]s?(x)'],
      extends: ['plugin:jest-dom/recommended', 'plugin:testing-library/react'],
    },
  ],
  settings: {
    react: {
      version: 'detect',
    },
  },
};
```

`tsconfig.json`を以下のように編集

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "useDefineForClassFields": true,
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
    "allowJs": false,
    "skipLibCheck": true,
    "esModuleInterop": false,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": ["src", ".eslintrc.cjs", "vite.config.ts"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

### prettier のインストール

自動でコードを整形してくれる prettier をインストールします。

```sh
npm i -D prettier eslint-config-prettier
```

完了後直下に`.prettierrc`を作成し以下の内容を記載します。

```json
{
  "printWidth": 120,
  "singleQuote": true,
  "trailingComma": "all"
}
```

## まとめ

ここでは Vite ＋ React を使用した環境構築方法、ESLint、prettier の設定方法を学びました。
次回からは実際にアプリの構築をやっていきます。
