Next.js 開発環境用テンプレート。

## 構成

- Next.js
- Tailwind CSS
- Sass

- Eslint（Next.jsに付属）
- Prettier
- Stylelint

バージョンはpackage.jsonを確認してください。

エディターの設定は各自で設定してください。

参考としてVSCodeの設定を載せています。

## Next.js

```bash
npx create-next-app@latest --ts
```

    "next": "12.1.0",
    "react": "17.0.2",
    "react-dom": "17.0.2"

    "@types/node": "17.0.18",
    "@types/react": "17.0.39",
    "eslint": "8.9.0",
    "eslint-config-next": "12.1.0",
    "typescript": "4.5.5"

### srcフォルダを作りpagesとstylesを移動

```
├── src
│   ├── pages
│   │   ├── _app.tsx
│   │   ├── api
│   │   │   └── hello.ts
│   │   └── index.tsx
│   └── styles
│       ├── Home.module.scss
│       └── globals.scss
```

```diff
--package.json
  "scripts": {
+    "lint": "next lint --dir src",
-    "lint": "next lint",
  },
```

### パスのalias

```js
//tsconfig.json
{
  "compilerOptions": {
  
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
  },
  
}
```

```diff
+ import "@/styles/global.scss";
- import '../styles/globals.scss'
```

## Sass

### install

```bash
npm i sass -D
```

    "sass": "^1.49.8",

### 拡張子変更

```diff
//ファイル名変更
+ globals.scss
+ Home.module.scss

- globals.css
- Home.module.scss
```

## Tailwind CSS

### install

```bash
npm i -D tailwindcss postcss autoprefixer
```

    "autoprefixer": "^10.4.2",
    "postcss": "^8.4.6",
    "tailwindcss": "^3.0.23",

### 設定

```bash
npx tailwindcss init -p
```

```js
//tailwind.config.js
module.exports = {
  mode: 'jit',
  content: [
    '.src/pages/**/*.{js,ts,jsx,tsx}',
    '.src/components/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

```css
//globals.scss
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Prettier, plugin for Tailwind CSS

### install

```bash
npm install -D prettier prettier-plugin-tailwindcss eslint-config-prettier
```

    "prettier": "^2.5.1",
    "prettier-plugin-tailwindcss": "^0.1.7",
    "eslint-config-prettier": "^8.4.0",

### 設定

参考：[Prettier Options](https://prettier.io/docs/en/options.html)

```js
//.prettierrc.json
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": false,
  "quoteProps": "as-needed",
  "jsxSingleQuote": false,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "always",
  "proseWrap": "preserve",
  "htmlWhitespaceSensitivity": "css",
  "endOfLine": "lf",
  "embeddedLanguageFormatting": "off"
}
```

```js
//.prettierignore
*.md
```

```js
//.eslintrc.json
{
  "extends": ["next/core-web-vitals", "prettier"]
}
```

### 【参考】vscode拡張機能設定

拡張機能：[Prettier Formatter for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

```js
//settings.json
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[markdown]": {
    "editor.formatOnSave": false
  },
  "eslint.validate": ["html", "javascriptreact", "typescriptreact"],
```

マークダウンファイルをprettierで整形すると、全角文字と半角文字の間に全てスペースが入るため、マークダウンを除外している。

`.prettierignore`だけあれば`setting.json`の記述はなくてもvscodeでファイル保存時にマークダウンは無視される。

ただし、vscodeのコマンド（ctrl + shift + pで選択できるもの）を使うと、設定をしていても整形される。


## Stylelint

参考：[Stylelint Getting started](https://github.com/stylelint/stylelint/blob/main/docs/user-guide/get-started.md)

```bash
npm i stylelint stylelint-config-prettier-scss stylelint-config-standard-scss stylelint-config-recess-order -D
```

```js
//.stylelintrc.json
{
  "extends": [
    "stylelint-config-standard-scss",
    "stylelint-config-recess-order",
    "stylelint-config-prettier-scss"
  ],
  "rules": {
    "scss/at-rule-no-unknown": [
      true,
      {
        "ignoreAtRules": ["apply", "layer", "responsive", "screen", "tailwind"]
      }
    ]
  }
}
```

### 【参考】vscode拡張機能設定



拡張機能：[Stylelint : VSCode Marketplace](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint)

```js
//settings.json
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[markdown]": {
    "editor.formatOnSave": false
  },
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.fixAll.stylelint": true
  },
  "eslint.validate": ["html", "javascriptreact", "typescriptreact"],
  "css.validate": false,
  "scss.validate": false,
  "stylelint.enable": true,
  "stylelint.validate": ["css", "scss"],
```

```js
//package.json
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint --dir src",
    "slint": "stylelint **/*.{css,scss,sass}"
  },
```

## 詳細

- Next.js
  - create-next-appをTypeScriptで
  - srcフォルダ作成
  - package.json"scripts"のlintコマンドを修正
  - tsconfig.jsonへパスのalias設定を追加
- Sass
  - cssファイルの拡張子をscssに変更
- TailwindCSS
  - tailwind.config.jsを作成
    - JITモード追加
    - contentに該当ファイルを指定
- Eslint
  - .eslintrc.jsonにPrettierを追加
- Prettier
  - Tailwind CSS用のプラグインを追加
  - Prettierと競合するEslintのルールをオフにする設定を追加
  - .prettierrc.jsonを作成
  - .prettierignoreを作成
    - マークダウンファイルを除外設定
- Stylelint
  - Scss用の設定を追加
  - prettierと競合するStylelintのルールをオフにする設定を追加
  - cssのプロパティを自動で並び変える設定を追加
  - package.jsonにコマンドを追加
  - .stylelintrc.jsonを作成
    - 各種プラグインを設定
    - Tailwind CSSの除外設定
