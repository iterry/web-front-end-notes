### 简介

PostCSS是一个工具，一个使用JS工具和插件转换CSS代码的工具；也是一套插件体系。为什么这么说呢？

PostCSS本身就做了一件事：将CSS/CSS-Processor代码解析成AST（Abstract Syntax Tree，抽象语法树），其余对CSS的操作都是基于AST由插件进行的处理，比如：添加兼容前缀（Autoprefixer）、CSS新特性优雅降级（Preset Env）、CSS Modules等等。而这些插件都要基于AST以及PostCSS提供的plugin方法来定义。

> > 抽象语法树指的是源代码语法对应的树状结构，代码语句和符号映射到树中的每一个节点。AST具有两个特点：
> >
> > 一、不依赖具体文法。文法指的是一门编程语言的语法规则描述规范，AST的构建不依赖源语言的文法，有其自己的一套规范，这也保证了不同语言的文法，在语法分析的时候，构造出相同的语法树。
> >
> > 二、不依赖语言的细节。不同的编程语言有各自的语法，比如：流程控制if - elseif -else等等，AST在构建的时候，只保留了分支节点，对程序中出现的括号、关键字，通通去掉。
> >
> > 就WEB领域，AST在编译器、浏览器渲染解析、IDE、流程构建工具、代码优化压缩等多个地方有广泛应用。
> >
> > 比如：UglifyJS压缩JS代码实质上也是对JS代码的AST进行的操作，之后再重新生成压缩后的JS源码。
> >
> > 其中，JS的语法解析器[Espsrima](http://esprima.org/)（[http://esprima.org/demo/parse.html）](http://esprima.org/demo/parse.html）) 提供了在线的AST解析工具，可以查看AST结构，在此不做深入扩展。

### 先来了解插件的写法。

一个插件是一个Node.js的模块，其名称以“postcss-”作为前缀，插件还需要一个进行初始化的方法。该方法的参数是插件所支持的配置选项，而返回值则是另外一个方法，用来进行实际的处理。该处理方法会接受两个参数，css 代表的是表示 CSS AST 的对象，而 result 代表的是处理结果。**如下示例**，该插件使用 css 对象的 walkDecls 方法来遍历所有的“color”属性声明，并对“color”属性值进行检查。如果属性值为 black，就使用 result 对象的 warn 方法添加一个警告消息。

```
 return function(css, result) {
   css.walkDecls('color', function(decl) {
     if (decl.value == 'black') {
       result.warn('No black color.', {decl: decl});
     }
   });
 };
})
```

PostCss不单独使用，而是与已有的构建工具集成使用，如：Webpack、Gulp等等。集成完毕，选择配置需要的插件，即可使用。

### 下面介绍Webpack和Gulp中使用插件的示例：

在Webpack中的配置：

```
module.exports = {
 context: path.join(__dirname, 'app'),
 entry: './app',
 output: {
   path: path.join(__dirname, 'dist'),
   filename: 'bundle.js'
 },
 module: {
   loaders: [
     {
       test:   /\.css$/,
       loader: "style-loader!css-loader!postcss-loader"
     }
   ]
 },
 postcss: function () {
   return [require('autoprefixer')];
 }
}
```

在Gulp中的配置：

```
var gulp = require('gulp');

gulp.task('css', function() {
 var postcss = require('gulp-postcss');
 return gulp.src('app/**/*.css')
   .pipe(postcss([require('autoprefixer')]))
   .pipe(gulp.dest('dist/'));
});
```



