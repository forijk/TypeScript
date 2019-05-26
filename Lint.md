# Lint 代码检查

- 目前 TypeScript 的代码检查主要有两个方案：使用 TSLint 或使用 ESLint + typescript-eslint-parser。
- TSLint 与 ESLint 类似，不过除了能检查常规的 js 代码风格之外，TSLint 还能够通过 TypeScript 的语法解析，利用类型系统做一些 ESLint 做不到的检查。

- TSLint 与 ESLint 作为检查 TypeScript 代码的工具，各自有各自的优点：

- TSLint 的优点：

> 专为 TypeScript 服务，bug 比 ESLint 少
> 不受限于 ESLint 使用的语法树 ESTree​
> 能直接通过 tsconfig.json 中的配置编译整个项目，使得在一个文件中的类型定义能够联动到其他文件中的代码检查

- ESLint 的优点：

> 基础规则比 TSLint 多很多（249 : 151）
> 社区繁荣，插件众多（50+ : 9）

- 下面来看一些具体的例子：

let foo: string = 1 + '1';
​
// tslint 报错信息：
//
// ERROR: /path/to/index.ts[1, 19]: Operands of '+' operation must either be both strings or both numbers, consider using template literals
以上代码在 TSLint 中会报错，原因是加号两边必须同为数字或同为字符串（需要开启 restrict-plus-operands 规则）。

- ESLint 无法知道加号两边的类型，所以对这种规则无能为力。

### 在 TypeScript 中使用 ESLint

安装 ESLint

- ESLint 可以安装在当前项目中或全局环境下，因为代码检查是项目的重要组成部分，所以我们一般会将它安装在当前项目中。可以运行下面的脚本来安装：

- npm install eslint --save-dev
- 由于 ESLint 默认使用 Espree 进行语法解析，无法识别 TypeScript 的一些语法，故我们需要安装 typescript-eslint-parser，替代掉默认的解析器，别忘了同时安装 typescript：
- npm install typescript typescript-eslint-parser --save-dev
- 由于 typescript-eslint-parser 对一部分 ESLint 规则支持性不好，故我们需要安装 eslint-plugin-typescript，弥补一些支持性不好的规则。
- npm install eslint-plugin-typescript --save-dev

### 检查整个项目的 ts 文件

- 我们的项目源文件一般是放在 src 目录下，所以需要将 package.json 中的 eslint 脚本改为对一个目录进行检查。由于 eslint 默认不会检查 .ts 后缀的文件，所以需要加上参数 --ext .ts：

```javascript
{
    "scripts": {
        "eslint": "eslint src --ext .ts"
    }
}
```

- 此时执行 npm run eslint 即会检查 src 目录下的所有 .ts 后缀的文件。

- VSCode 中的 ESLint 插件默认是不会检查 .ts 后缀的，需要在「文件 => 首选项 => 设置」中，添加以下配置：

```javascript
{
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "typescript"
    ]
}
```

### 使用 ESLint 检查 tsx 文件

如果需要同时支持对 tsx 文件的检查，则需要对以上步骤做一些调整：

安装 eslint-plugin-react

```javascript
npm install --save-dev eslint-plugin-react
package.json 中的 scripts.eslint 添加 .tsx 后缀

{
    "scripts": {
        "eslint": "eslint src --ext .ts,.tsx"
    }
}
VSCode 的配置中新增 typescriptreact 检查

{
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "typescript",
        "typescriptreact"
    ]
}
```

### 在 TypeScript 中使用 TSLint

TSLint 的使用比较简单，参考官网的步骤安装到本地即可：

npm install --save-dev tslint
创建配置文件 tslint.json

```javascript
{
    "rules": {
        // 必须使用 === 或 !==，禁止使用 == 或 !=，与 null 比较时除外
        "triple-equals": [
            true,
            "allow-null-check"
        ]
    },
    "linterOptions": {
        "exclude": [
            "**/node_modules/**"
        ]
    }
}
为 package.json 添加 tslint 脚本

{
    "scripts": {
        "tslint": "tslint --project . src/**/*.ts src/**/*.tsx",
    }
}
```

其中 --project . 会要求 tslint 使用当前目录的 tsconfig.json 配置来获取类型信息，很多规则需要类型信息才能生效。

此时执行 npm run tslint 即可检查整个项目。

### 在 VSCode 中集成 TSLint 检查

- 在 VSCode 中安装 tslint 插件即可，安装好之后，默认是开启的状态。

### 使用 AlloyTeam 的 TSLint 配置

AlloyTeam 为 TSLint 也打造了一套配置 tslint-config-alloy​

npm install --save-dev tslint-config-alloy
安装之后修改 tslint.json 即可

```javascript
{
    "extends": "tslint-config-alloy",
    "rules": {
        // 这里填入你的项目需要的个性化配置，比如：
        //
        // 一个缩进必须用两个空格替代
        // "indent": [
        //     true,
        //     "spaces",
        //     2
        // ]
    },
    "linterOptions": {
        "exclude": [
            "**/node_modules/**"
        ]
    }
}
```

### 使用 TSLint 检查 tsx 文件

TSLint 默认支持对 tsx 文件的检查，不需要做额外配置
