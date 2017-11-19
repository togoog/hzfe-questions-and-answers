## 简单实现
```javascript
function map (arr, func) {
  let result = []
  for(let i = 0, l = arr.length; i<l; i++) {
    result[i] = func(arr[i], i, arr)
  }
  return result
}
```

## jQuery 实现
```javascript
// 以下map函数中使用了jQuery其他方法的地方 这里做了修改(为了减少篇幅)
// 不影响原意和map的理解 可以直接在控制台跑
// arg is for internal usage only
function map ( elems, callback, arg ) {
  var length, value,
    i = 0,
    ret = [];

  // 如果是(类)数组：遍历数组，将每个项目转换为新值
  if ( elems instanceof Array ) {
    length = elems.length;
    for ( ; i < length; i++ ) {
      value = callback( elems[ i ], i, arg );

      if ( value != null ) {
        ret.push( value );
      }
    }

  // 遍历对象上的可枚举属性
  } else {
    for ( i in elems ) {
      value = callback( elems[ i ], i, arg );

      if ( value != null ) {
        ret.push( value );
      }
    }
  }

  // 拍平嵌套数组
  return [].concat.apply( [], ret );
}
```

## underscore 实现
```javascript
// 通过对list里的每个元素调用转换函数(iteratee迭代器)生成一个与之相对应的数组。
// iteratee传递三个参数：value，index(如果list是个JavaScript对象是，则该参数为key)，list
_.map = _.collect = function(obj, iteratee, context) {

  // iteratee参数可能是函数 || 对象 || 字符串，underscore 的内置函数 cb 将其统一处理为一个函数。
  // cb详细解析可以看 https://github.com/Akiq2016/underscore/blob/master/underscore.js#L91
  iteratee = cb(iteratee, context);

  // 表示给定对象的所有可枚举属性的字符串数组。 [ isArrayLike判断是否(类)数组，_.keys实现Object.keys ]
  var keys = !isArrayLike(obj) && _.keys(obj),
      length = (keys || obj).length,
      results = Array(length);

  // 遍历数组 || 遍历对象上的可枚举属性, 将每个项目转换为新值
  for (var index = 0; index < length; index++) {
    var currentKey = keys ? keys[index] : index;
    results[index] = iteratee(obj[currentKey], currentKey, obj);
  }

  return results;
};
```