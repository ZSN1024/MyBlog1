## TypeScript 基础
本部分介绍了 TypeScript 中的常用类型和一些基本概念，旨在让大家对 TypeScript 有个初步的理解

## 安装

TypeScript 的命令行工具安装方法如下：

```bash
npm install -g typescript
```

以上命令会在全局环境下安装 `tsc` 命令，安装完成之后，我们就可以在任何地方执行 `tsc` 命令了。

我们约定使用 TypeScript 编写的文件以 `.ts` 为后缀，用 TypeScript 编写 React 时，以 `.tsx` 为后缀。

现在我们可以新建一个test.ts文件，并写入下面代码

```ts
var message:string = "Hello World"
console.log(message)
```
然后使用下面命令将TypeScript转换为JavaScript代码

```bash
tsc test.ts
```
这时候在当前目录下（与 test.ts 同一目录）就会生成一个 test.js 文件，代码如下：

```js
var message = "Hello World";
console.log(message);
```
我们可以使用node命令来执行test.js

```bash
$ node test.js

Hello World
```
## 编辑器

TypeScript 最大的优势便是增强了编辑器和 IDE 的功能，包括代码补全、接口提示、跳转到定义、重构等。

主流的编辑器都支持 TypeScript，这里我推荐使用 [Visual Studio Code](https://code.visualstudio.com/)。

它是一款开源，跨终端的轻量级编辑器，内置了 TypeScript 支持。

另外它本身也是[用 TypeScript 编写的](https://github.com/Microsoft/vscode/)。

下载安装：https://code.visualstudio.com/

获取其他编辑器或 IDE 对 TypeScript 的支持：

- [Sublime Text](https://github.com/Microsoft/TypeScript-Sublime-Plugin)
- [Atom](https://atom.io/packages/atom-typescript)
- [WebStorm](https://www.jetbrains.com/webstorm/)
- [Vim](https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support#vim)
- [Emacs](https://github.com/ananthakumaran/tide)
- [Eclipse](https://github.com/palantir/eclipse-typescript)
- [Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48593)
- [Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=48739)

---
## 基础语法
### 基础类型
### 任意值
### 类型推论
### 联合类型
### 对象类型-接口
### 数组类型
### 函数Function
### 类型断言
### 声明文件
### 内置对象