#### 1、三种及以上方法合并两个一位数值。

```js
var arr1 = [1,2,3],
    arr2 = [4,5,6];

// arr1 = [1,2,3,4,5,6]
arr1.concat(arr2);

// arr1 = [1,2,3,4,5,6]
Array.prototype.push(arr1, arr2);

// arr1 = [1,2,3,4,5,6]
arr1.push(...arr2);

// arr1 = [1,2,3,4,5,6]
arr1.splice(arr1.length, 0, ...arr2)

// arr3 = [1,2,3,4,5,6]
arr3 = [...a1, ...a2]

// arr3 = [1,2,3,4,5,6]
arr3 = Array.of(...arr1, ...arr2)
```

#### 2、实现一个函数，满足如下结果：
参考：[JS类型转换的思考](http://www.cnblogs.com/coco1s/p/6509141.html)

```
add(1) //1
add(1)(2) //3
add(1)(2)(3)(4) //10
```

```
function add(){
    var args = Array.prototype.slice.call(arguments);
    var fn = function(){
        var args1 = Array.prototype.slice.call(arguments);
        return add.apply(null, args.concat(args1));
    }
    // 关键是理解valueOf()会在函数调用的时候自行调用
    fn.valueOf = function(){
        return args.reduce((prev, curr) => {
            return prev+curr;
        })
    }
    return fn;
}
```



