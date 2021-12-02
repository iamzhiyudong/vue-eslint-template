# 集成工程化规范相关的工具

<a name="r5H2I"></a>

### 1. ESLint

​<br />
<a name="uZeaE"></a>

#### 1. 安装初始化

​

首先在项目目录中安装 ESLint

> npm i -D eslint

​

安装成功之后，初始化 eslint，执行以下命令，并根据提示选择相应的配置，之后会在项目的根目录里生成 .eslintrc 配置文件

> ./node_modules/.bin/eslint --init

​

在安装过程中选择 Vue 类型的项目后，自动安装的包如下:

```json
"eslint": "^8.3.0",
"eslint-plugin-vue": "^8.1.1", // 让 ESLint 支持检测 Vue 语法 [https://eslint.vuejs.org/]
```

默认生成的** .eslintrc.json **文件如下：

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": [
    "eslint:recommended", // 启用推荐的规则，报告一些常见的问题 [https://cn.eslint.org/docs/rules/]
    "plugin:vue/essential" // 主要继承了 eslint-plugin-vue 插件，支持同时检查你 .vue 文件中的模板和脚本 [https://vue-loader-v14.vuejs.org/zh-cn/workflow/linting.html]
  ],
  "parserOptions": {
    "ecmaVersion": 13,
    "sourceType": "module"
  },
  "plugins": ["vue"],
  "rules": {}
}
```

> ESLint 的 plugins 和 extends：
>
> 1. **plugins**：插件主要是为 eslint 新增一些检查规则，，举个例子：eslint-plugin-react 就会对 react 项目做了一些定制的 eslint 规则
> 1. **extends**：extends 可以看做是去集成一个个配置方案的最佳实践，它配置的内容实际就是一份份别人配置好的.eslintrc.js。以上述的 eslint-plugin-react 为例，它实现了几十种配置规则，为了方便其他人使用，它默认实现了两种最佳实践 all 以及 recommened（在 configs 中可以看到具体的名称）
>
> [【掘金】ESLint 中 plugins 和 extends 的区别](https://juejin.cn/post/6859291468138774535)

​

<a name="GdLHu"></a>

#### 2. 配置

<br />在这里我们使用 airbnb 规范，用到的 config 是 `eslint-config-airbnb-base`，参考一篇[文章](https://agm1984.medium.com/how-to-set-up-es-lint-for-airbnb-vue-js-and-vs-code-a5ef5ac671e8)。<br />注：这里我们使用的是 airbnb-base，是因为当前项目是 Vue，如果是 React 项目的话使用 airbnb 就行<br />​

先根据 npm 官网上提供的步骤，检查安装所需要的正确版本依赖，把缺的安装上(`eslint-plugin-import`)。

> npm info "eslint-config-airbnb-base@latest" peerDependencies
> yarn add -D eslint-config-airbnb-base

​

安装 `@babel/eslint-parser`，并在配置文件中添加配置：

```json
"parser": "@babel/eslint-parser",
```

> **关于@babel/eslint-parser:** (babel-eslint 不再维护和更新)
>
> 1. ESLint 的默认解析器和核心规则仅支持最新的最终 ECMAScript 标准，不支持 Babel 提供的实验性（例如新功能）和非标准（例如 Flow 或 TypeScript 类型）语法。
> 1. @ babel / eslint-parser 是允许 ESLint 在由 Babel 转换的源代码上运行的解析器。
> 1. 当你使用 babel 时，babel 的解析器会把你的 code 转换为 AST，该解析器会将其转换为 ESLint 能懂的 ESTree。

​

​

​

最后配置好的 **.eslintrc.json **文件如下：

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": ["airbnb-base", "plugin:vue/essential"], // eslint 默认提供的就可以不要了
  "parserOptions": {
    "parser": "@babel/eslint-parser",
    "ecmaVersion": 13,
    "sourceType": "module"
  },
  "plugins": ["vue"],
  "rules": {}
}
```

<a name="lpgEb"></a>

### 2. Prettier

​<br />
<a name="ySNZL"></a>

#### 1. 安装

共需要安装 3 个包：

> 1. prettier
> 1. eslint-config-prettier: 关闭了可能与 Prettier 冲突的所有 ESLint 规则
> 1. eslint-plugin-prettier: 将 Prettier 规则集成到 ESLint 规则中, 将差异转换为单个的 Eslint 问题
>
> 它们负责将 prettier 和 eslint 组合起来

​<br />

> npm install -D prettier eslint-config-prettier eslint-plugin-prettier

​

​<br />
<a name="CNjJO"></a>

#### 2. 配置

<br />**组合 prettier 和 eslint **<br />**​**

配置 **.eslintrc**， 如果需要从 ESLint 规则中排除 文件夹/文件，可以将它们添加到 .eslintignore 文件中（例如`node_modules / *`，将一下配置合并到 **.eslintrc **文件中。

```json
{
  "extends": ["prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": ["error"] // 表示被 prettier 标记的地方抛出错误信息
  }
}
```

目前的 **.eslintrc.json **

```json
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": ["airbnb-base", "plugin:vue/recommended", "prettier"],
  "parserOptions": {
    "parser": "@babel/eslint-parser",
    "ecmaVersion": 2017,
    "sourceType": "module"
  },
  "plugins": ["vue", "prettier"],
  "rules": { "prettier/prettier": ["error"], "import/no-unresolved": "off" }
}
```

<br />
<br />**配置 prettier**<br />**​**

**.prettierrc**

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

​

到这里编辑器就能正常的提示不规范的地方了。<br />​<br />
<a name="YvQPu"></a>

### 3. Stylelint

<a name="AX2qY"></a>

#### 1. 安装

共需要安装 5 个包 [[参考文章](https://juejin.cn/post/6878121082188988430)]：

> 1. stylelint
> 1. stylelint-config-prettier： 禁用所有与格式相关的 Stylelint 规则，解决 prettier 与 stylelint 规则冲突
> 1. stylelint-config-standard：官网提供的 css 标准
> 1. stylelint-no-unsupported-browser-features：检查正在使用的 CSS 是否受目标浏览器支持
> 1. stylelint-prettier：基于 prettier 代码风格的 stylelint 规则

​<br />

> yarn add -D stylelint stylelint-config-prettier stylelint-config-standard stylelint-no-unsupported-browser-features stylelint-prettier

<br />配置如下 **stylelint.config.js**

```javascript
module.exports = {
  extends: ['stylelint-config-standard', 'stylelint-config-prettier'],
  plugins: ['stylelint-prettier', 'stylelint-no-unsupported-browser-features'],
  rules: {
    'prettier/prettier': true,
    'plugin/no-unsupported-browser-features': [
      true,
      {
        severity: 'warning',
      },
    ],
  },
}
```

**.browserslistrc **配置需要支持的浏览器的版本信息

```javascript
> 1%
last 2 versions
not ie <= 8
Firefox >= 2
```

​

​<br />
<a name="eBsde"></a>

### 4. Husky & lint-stged

让我们在提交代码之前，对代码可以执行一些操作（ESLint / Prettier ... ）<br />安装 lint-staged 的时候，默认会一起吧 husky 安装上，所以不需要再单独安装了。<br />​<br />
<a name="bBks5"></a>

#### 1. 安装

按照 github 上的提示装就好

> npx mrm@2 lint-staged

​

安装完成后，在 package.json 文件的最后位置会默认添加一些 lint-staged 配置，删除掉就行。

```json
// 删除掉就行
"lint-staged": {
  "*.js": "eslint --cache --fix",
  "*.css": "stylelint --fix"
}
```

<a name="KUIiG"></a>

#### 2. 配置

<br />根目录新建 **.lintstagedrc **文件，并下边的配置写进去

```json
{
  "src/**/*.{js,vue}": ["eslint --cache --fix", "prettier --write"],
  "src/**/*.{css,less,scss,sass}": ["stylelint --fix", "prettier --write"],
  "*.md": ["prettier --write"]
}
```

​<br />
<a name="nR2Jk"></a>

##### 1. pre-commit

​

如果想在了 git commit 的时候执行以上操作，则需要在 husky 的 pre-commit.sh 里添加命令<br />​

在项目根目录的 .husky 目录下， 添加的内容如下：

```shell
npx lint-staged -c .lintstagedrc
# -c 表示指定 lint-staged 的配置文件
```

​<br />
<a name="TxlMn"></a>

##### 2. commit-msg

此时的项目中默认还没有给提供 husky 的 commit-msg 钩子，需要自己添加，根据[官网](https://typicode.github.io/husky/#/?id=automatic-recommended)描述可知：

```shell
npx husky add .husky/commit-msg 'npx commitlint -e'
```

执行以上命令后，在 .husky 的目录下就多了一个 sh 文件 commit-msg，在文件中可设置在执行 git commit 的时候需要执行的操作

> commitlint 为提交 git 的时候，对 commit message 的一个校验工具，在下文会提到

<a name="oyOcW"></a>

###

<a name="gM6kV"></a>

### 5. Commitlint

<br />根根据官方文档可知，需要安装如下：

> @commitlint/config-conventional
> @commitlint/cli

> yarn add -D @commitlint/config-conventional @commitlint/cli

​

在项目根目录添加 commitlint.config.js 文件，并填入一下内容：

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
}
```

​

到这里整个规范化的搭建基本上就结束了。<br />​<br />

---

参考

1. [【掘金】Prettier + ESLint 使用教程](https://juejin.cn/post/6844903917260636173)
1. [【Stack Overflow】How to configure Vue CLI 4 with ESLint + Airbnb rules + TypeScript + Stylelint for SCSS, in VS Code editor with autofix on save?](https://stackoverflow.com/questions/60187885/how-to-configure-vue-cli-4-with-eslint-airbnb-rules-typescript-stylelint-f)
1. [【掘金】项目中 Prettier + Stylelint + ESlint 配置](https://juejin.cn/post/6878121082188988430) 👍 🔥
1. [【GitHub】stylelint-prettier](https://github.com/prettier/stylelint-prettier)
1. [【NPM】stylelint-no-unsupported-browser-features](https://www.npmjs.com/package/stylelint-no-unsupported-browser-features)
1. [【Github】lint-staged](https://github.com/okonet/lint-staged#configuration)
1. [为你的项目添加 commitlint ](https://www.codeleading.com/article/24521762378/)​

---

## Project setup

```
yarn install
```

### Compiles and hot-reloads for development

```
yarn serve
```

### Compiles and minifies for production

```
yarn build
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
