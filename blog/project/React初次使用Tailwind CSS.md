# React初次使用Tailwind CSS

参考文档：https://www.tailwindcss.cn/docs/guides/create-react-app

## 创建工程

```bash
npm init react-app  my-first-app-with-tailwindcss
或者
npx create-react-app my-first-app-with-tailwindcss
```

## 初始化Tailwind CSS

```
npm install -D tailwindcss@npm:@tailwindcss/postcss7-compat postcss@^7 autoprefixer@^9
npm install @craco/craco
```

安装完毕后，更新 `package.json` 文件中的 `scripts`，将 `eject` 以外的所有脚本都用 `craco` 代替 `react-scripts`。

```json
"scripts": {
    // "start": "react-scripts start",
    // "build": "react-scripts build",
    // "test": "react-scripts test",
    // "eject": "react-scripts eject",
    "start": "craco start",
    "build": "craco build",
    "test": "craco test"
  },
```

接下来，在项目根部创建一个 `craco.config.js`，并添加 `tailwindcss` 和 `autoprefixer `作为 PostCSS 插件。

```javascript
// craco.config.js
module.exports = {
  style: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
}
```

## 创建您的配置文件

```bash
npx tailwindcss-cli@latest init
```

这将会在您的项目根目录创建一个最小化的 `tailwind.config.js` 文件：

```javascript
module.exports = {
  purge: [],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
```

##  配置 Tailwind 来移除生产环境下没有使用到的样式声明

在您的 `tailwind.config.js` 文件中，配置 `purge` 选项指定所有的 components 文件，使得 Tailwind 可以在生产构建中对未使用的样式进行摇树优化。

```javascript
  // tailwind.config.js
  module.exports = {
   // purge: [],
   purge: ['./src/**/*.{js,jsx,ts,tsx}', './public/index.html'],
    darkMode: false, // or 'media' or 'class'
    theme: {
      extend: {},
    },
    variants: {
      extend: {},
    },
    plugins: [],
  }
```

##  在您的 CSS 中引入 Tailwind

打开 Create React App 默认为您生成的 ./src/index.css 文件 并使用 `@tailwind` 指令来包含 Tailwind的 `base`、 `components` 和 `utilities` 样式，来替换掉原来的文件内容。

