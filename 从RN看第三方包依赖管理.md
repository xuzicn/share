### 自我介绍


### 现阶段RN的进展和缺陷

###### 大规模应用之前，需要解决的缺陷
1. 单JSCore，跨部门合作
 - bundle拆分，react-native-packager改造
 - JSCore加载JS方式的改造，需要能够
 -

2. 没有合适的Router
3. 未有1.0版本，接口并非稳定
4. 包依赖众多，无法构成稳定的跨平台开发、打包、测试环境。

###### 官版基本原理和源代码结构
1. 一个框架，一个命令，三个NPM包
2. 单JSCore，单Bundle

针对问题1和2
这是NPM的锅，也是今天主要讨论的问题，我们先看看问题是怎么产生的。


### 推演包依赖问题的产生的原因
##### 单组应用
开始一个新的项目，基本上是这样的。第一个人建立起基本的项目模板：

    npm install react-native-cli -g
    react-native init AwesomeProject
    npm shrinkwrap
    git push

其他开发者：

    npm install react-native-cli -g
    git clone
    npm install

react-native init AwesomeProject这一步，约等于

    mkdir AwesomeProject && cd AwesomeProject
    npm install react-native --save
    copy模板（index.ios.js等）

简述一下npm shrinkwrap命令的作用，npm shrinkwrap在项目根目录下产生了一个npm-shrinkwrap.json文件，<b>这个文件锁定了整个项目依赖的第三方组件版本</b>。
因为npm上众多的源采用Semantic的版本标准，不可避免的出现这样的情况
> A@0.0.1 -> ^B@0.0.1

项目中运用了A@0.0.1的模块，依赖树可能是这样的
> project -> A@0.0.1 -> B@0.0.1

当B模块发布了0.0.2版本后，其他开发者重新clone代码仓库并且npm install之后，依赖树就变成
> project -> A@0.0.1 -> B@0.0.2

npm-shrinkwrap.json则可以避免这一问题的产生，因为npm install命令在构建依赖树时，package.json和npm-shrinkwrap.json文件都会产生影响，npm-shrinkwrap.json的优先级更高。

##### 跨团队合作

#####
