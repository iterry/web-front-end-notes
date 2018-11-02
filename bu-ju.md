#### 1、如何处理iphoneX的适配问题？

解决iphonex适配，需要了解屏幕的安全区域。

如下图所示，安全区域表示一个可视窗口范围，剔除掉了圆角、齐刘海、小黑条的影响：![](/assets/iphonex_fit.png)其中，浏览器已经对齐刘海区域做了处理。

适配方案如下：

```
// 1、viewport 添加viewport-fit=cover
// viewport-fit取contain/cover/auto等值，默认是 contain。需要适配的话需要将其设置为 cover
<meta name="viewport" content="width=device-width, viewport-fit=cover">

// 2、使用iOS11新添加的属性constant() 和 env() 做吸底处理
// iOS11新增的特性，Webkit的CSS函数，用于设定安全距离和边界的距离，取值：safe-area-inset-left/right/top/bottom
// 其中，constant()兼容iOS11.2之前，env()兼容iOS11.2及之后，书写顺序不能换，见下：
{
  margin-bottom: constant(safe-area-inset-bottom);
  margin-bottom: env(safe-area-inset-bottom);
}
或者
{
  bottom: calc(xxpx + constant(safe-area-inset-bottom));
  bottom: calc(xxpx + env(safe-area-inset-bottom));
}

```

当然，为了隔绝兼容样式，可以做如下处理：

```
@supports (bottom: constant(safe-area-inset-bottom)) or (bottom: env(safe-area-inset-bottom)) {
  div {
    margin-bottom: constant(safe-area-inset-bottom);
    margin-bottom: env(safe-area-inset-bottom);
  }
}
```



