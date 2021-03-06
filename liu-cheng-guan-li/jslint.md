JSLint是一个非开源的JS代码验证工具，通过扫描JS代码来查找问题，问题会以“描述+位置”的方式显示出来。

值得注意的是，JSLint约定的编码规范比ECMAScript本身更加严格：除了明显的语法错误，结构不符合最佳规范、缺少可以缺省的符号乃至代码缩进，都可能被JSLint指出来。因而，JSLint更加像一个严格的JS的编码规范，因为它遵从一个重要的编码宗旨：能做并不意味应该做。

由于 JSLint 工具本质上是一个普通的 JS 脚本，其运行也自然依赖于一个 JS 运行引擎，其被引擎加载后会在内存中产生一个全局 JSLint 函数对象，该函数对象需要两个输入量：source 和 options，前者用来指定待检测的脚本文件被解析后生成的字符串或字符串数组，后者则表示用户自定义的规则选项。若 options 为空，JSLint 则使用其默认的规则对 source 进行扫描检测。

JSLint由于及其严格的规范，被广泛集成在各种构建工具（Webpacj、Gulp、FIS、Ant）和编辑器插件（Sublimt、Vim、VSCode）。

