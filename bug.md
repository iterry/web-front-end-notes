#### 1、如何解决ios8+中使用 -webkit-overflow-scrolling:touch；出现的卡顿bug？

* 如果使用该属性的元素设置了定位，尝试不设置定位或者手动将定位设置为static；
* 如果是动态添加元素到滚动框内，通过内容撑开容器触发滚动，尝试以下方法：

```js
// 基本思路是，初始就将容器撑开，主动触发scrollbar
<div style="overflow:auto; -webkit-overflow-scrolling:touch;">
    <div style="min-height:101%">
    </div> 
    // or
    <div style="height: calc(100% + 1px)">
    </div>
</div>
```

#### 2、iOS微信浏览器和Safari中animation-play-state失效的解决方案？

现象：在ios11及以上的微信浏览器和Safari浏览器中，页面动画使用了transform属性，使用animation-play-state暂停动画不起作用

解决方案：由于移除 animation 后，动画执行到的效果会还原，所以给当前元素添加一个和当前元素同等大小的父元素，让父元素得到改变值即可。

```js
// Dom
<div class="wrap">
  <div class="needAnim"></div>
</div>

// Css
.playing {
  animation: circle 10s infinite linear;
}
@keyframes circle {
  100%{
    transform: rotate(360deg);
  }
}

// js,pause the animation
let $anim= $(".needAnim").css('transform')  //获取当前元素的动画改变，transform的值
let $par = $('.wrap').css('transform')
$('.wrap').css('transform',$par === 'none' ? $anim: $anim.concat('',$par))  
$img.removeClass('playing')
```



