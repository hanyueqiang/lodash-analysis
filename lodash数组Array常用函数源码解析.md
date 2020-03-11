

#### _.compact(array)
`compact`的释义为：紧凑的、简洁的，名词解释为契约
创建一个新数组，包含原数组中所有的非假值元素。例如false, null, 0, "", undefined, 和 NaN 都是被认为是“假值”。
```
/**
 * @static
 * @memberOf _
 * @since 0.1.0
 * @category Array
 * @param {Array} 待处理的数组.
 * @returns {Array} 返回过滤掉假值的新数组
 * @example
 *
 * _.compact([0, 1, false, 2, '', 3]);
 * // => [1, 2, 3]
 */

 /*
 *  解析:
 *  使用while循环遍历
 *  该函数使用的一个小技巧在于++i和i++的使用，++i先赋值后运算，所以进入while循环体内第一次赋值是index是0，而i++是先运算后赋值，resIndex 第一次是0参与运算，再自增1
 *
 */
function compact(array) {
  var index = -1,
      length = array == null ? 0 : array.length,
      resIndex = 0,
      result = [];

  while (++index < length) {
    var value = array[index];
    if (value) {
      result[resIndex++] = value;
    }
  }
  return result;
}

module.exports = compact;

练习题：请写一个过滤掉undefined、null、false、''、NaN假值元素且不包含0,返回一个不改变原数组的新数组？

function compact1(array) {
  let index = -1;
  let resIndex = 0;
  let length = array === null ? 0 : array.length;
  let result = [];

  while (++index < length) {
    let value = array[index];
    if (value || value === 0) {
      result[resIndex++] = value;
    }
  }
  return result;
}
let arr = [1, 0, '', null, 2, undefined, 'hello', false, NaN];
let newArr = compact1(arr); // [1, 0, 2, "hello"]

```
#### _.drop(array, [n=1])
`drop`释义为落下、减少
创建一个切片数组，去除array前面的n个元素。（n默认值为1。）
```
var baseSlice = require('./_baseSlice'),
    toInteger = require('./toInteger');

/**
 * 创建一个 `array`，从头开始删除 `n` 个元素
 * 解析：n是从数组个数，不是索引
 * 利用原生Array.slice(start, end) 返回一个新数组，从开始位置到结束位置（不包括）
 * @static
 * @memberOf _
 * @since 0.5.0
 * @category Array
 * @param {Array} array The array to query.
 * @param {number} [n=1] The number of elements to drop.
 * @param- {Object} [guard] Enables use as an iteratee for methods like `_.map`.
 * @returns {Array} Returns the slice of `array`.
 * @example
 *
 * _.drop([1, 2, 3]);
 * // => [2, 3]
 *
 * _.drop([1, 2, 3], 2);
 * // => [3]
 *
 * _.drop([1, 2, 3], 5);
 * // => []
 *
 * _.drop([1, 2, 3], 0);
 * // => [1, 2, 3]
 */
function drop(array, n, guard) {
  var length = array == null ? 0 : array.length;
  if (!length) {
    return [];
  }
  n = (guard || n === undefined) ? 1 : toInteger(n);
  return baseSlice(array, n < 0 ? 0 : n, length);
}

module.exports = drop;

练习题：请实现一个切片数组，去除array前面的n个元素,注意不传n保持原数组元素返回;

function drop1(array, n) {
  let length = array === null ? 0 : array.length;
  if (!length) {
    return [];
  }
  n = n === undefined ? 0 : parseInt(n);
  if (n < 0) {
    n = -n > length ? 0 : (length + n);
  }
  return array.slice(n, length);
}
let arr1 = [1, 2, 3, 4, 5];
let newArr1 = drop1(arr1); // [1, 2, 3, 4, 5]
let newArr2 = drop1(arr1, 2);  // [3, 4, 5]
let newArr3 = drop1(arr1, -2); // [4, 5]
```
#### _.dropRight(array, [n=1])
创建一个切片数组，去除array尾部的n个元素。（n默认值为1。）
```
var baseSlice = require('./_baseSlice'),
    toInteger = require('./toInteger');

/**
 * 解析：同drop函数原理一样，还是使用slice操作数组，drop是计算第一个参数起始位置start,dropRight思想是去掉数组后面的，计算end位置
 * @static
 * @memberOf _
 * @since 3.0.0
 * @category Array
 * @param {Array} array The array to query.
 * @param {number} [n=1] The number of elements to drop.
 * @param- {Object} [guard] Enables use as an iteratee for methods like `_.map`.
 * @returns {Array} Returns the slice of `array`.
 * @example
 *
 * _.dropRight([1, 2, 3]);
 * // => [1, 2]
 *
 * _.dropRight([1, 2, 3], 2);
 * // => [1]
 *
 * _.dropRight([1, 2, 3], 5);
 * // => []
 *
 * _.dropRight([1, 2, 3], 0);
 * // => [1, 2, 3]
 */
function dropRight(array, n, guard) {
  var length = array == null ? 0 : array.length;
  if (!length) {
    return [];
  }
  n = (guard || n === undefined) ? 1 : toInteger(n);
  n = length - n;
  return baseSlice(array, 0, n < 0 ? 0 : n);
}

module.exports = dropRight;

练习题：实现一个切片数组，取出后面的n个元素，不传n保持原数组组放回

function dropRight1(array, n) {
  let length = array === null ? 0 : array.length;
  if (!length) {
    return [];
  }
  n = n === undefined ? 0 : parseInt(n);
  n = length - n; // 此时n是传给slice的end用的，如果n是负数也符合，会返回原数组
  return array.slice(0, n);
}

```
#### _.indexOf(array, value, [fromIndex=0])
使用 SameValueZero 等值比较，返回首次 value 在数组array中被找到的 索引值， 如果 fromIndex 为负值，将从数组array尾端索引进行匹配。
```
var baseIndexOf = require('./_baseIndexOf'),
    toInteger = require('./toInteger');

/* Built-in method references for those with the same name as other `lodash` methods. */
var nativeMax = Math.max;

/**
 * array (Array): 需要查找的数组。
 * value (*): 需要查找的值。
 * [fromIndex=0] (number): 开始查询的位置。
 * @example
 *
 * _.indexOf([1, 2, 1, 2], 2);
 * // => 1
 *
 * // Search from the `fromIndex`.
 * _.indexOf([1, 2, 1, 2], 2, 2);
 * // => 3
 */
function indexOf(array, value, fromIndex) {
  var length = array == null ? 0 : array.length;
  if (!length) {
    return -1;
  }
  var index = fromIndex == null ? 0 : toInteger(fromIndex);
  if (index < 0) {
    index = nativeMax(length + index, 0);
  }
  return baseIndexOf(array, value, index);
}

module.exports = indexOf;

练习题：实现一个给出任一元素返回在该数组的下标，可添加起始查找位置,找不到返回-1
思路：使用js原生indexOf(searchvalue,fromindex)

function indexOf1(array, value, index) {
  if (!Array.isArray(array)) {
    return -1;
  }
  return array.indexOf(value, index);
}

```
#### _.flatten(array)
减少一级array嵌套深度。
```
var baseFlatten = require('./_baseFlatten');

/**
 * 解析：首先判断array是否是null或undefined,注意使用的是==,如果是===就是false要注意这点；
 * 解析：判断是数组的话调用baseFlatten(array, 1) 注意参数1是展开一层。
 * @static
 * @memberOf _
 * @since 0.1.0
 * @category Array
 * @param {Array} array The array to flatten.
 * @returns {Array} Returns the new flattened array.
 * @example
 *
 * _.flatten([1, [2, [3, [4]], 5]]);
 * // => [1, 2, [3, [4]], 5]
 */
function flatten(array) {
  var length = array == null ? 0 : array.length;
  return length ? baseFlatten(array, 1) : [];
}

module.exports = flatten;

/*
* 查看baseFlatten源码,
* while循环，如果depth>1进行递归调用，否则调用arrPush函数展开一层
* isStrict 真值检测，function函数会被过滤掉
*/
function baseFlatten(array, depth, predicate, isStrict, result) {
  var index = -1,
      length = array.length;

  predicate || (predicate = isFlattenable);
  result || (result = []);

  while (++index < length) {
    var value = array[index];
    if (depth > 0 && predicate(value)) {
      if (depth > 1) {
        // Recursively flatten arrays (susceptible to call stack limits).
        baseFlatten(value, depth - 1, predicate, isStrict, result);
      } else {
        arrayPush(result, value);
      }
    } else if (!isStrict) {
      result[result.length] = value;
    }
  }
  return result;
}

module.exports = baseFlatten;

```
#### _.uniq(array)
创建一个去重后的array数组副本。使用了 `SameValueZero` 做等值比较。只有第一次出现的元素才会被保留。
```
var baseUniq = require('./_baseUniq');

/**
 * 解析：先判断是否存在array,调用`baseUniq`实现。
 *
 * @static
 * @memberOf _
 * @since 0.1.0
 * @category Array
 * @param {Array} array The array to inspect.
 * @returns {Array} Returns the new duplicate free array.
 * @example
 *
 * _.uniq([2, 1, 2]);
 * // => [2, 1]
 */
function uniq(array) {
  return (array && array.length) ? baseUniq(array) : [];
}

module.exports = uniq;

```
#### _.without(array, [values])
创建一个剔除所有给定值的新数组，剔除值的时候，使用`SameValueZero`做相等比较。
注意: 不像`_.pull`, 这个方法会返回一个新数组。

```
var baseDifference = require('./_baseDifference'),
    baseRest = require('./_baseRest'),
    isArrayLikeObject = require('./isArrayLikeObject');

/**
 * 判断是否是数组或类数组，调用baseDifference
 * @memberOf _
 * @since 0.1.0
 * @category Array
 * @param {Array} array The array to inspect.
 * @param {...*} [values] The values to exclude.
 * @returns {Array} Returns the new array of filtered values.
 * @see _.difference, _.xor
 * @example
 *
 * _.without([2, 1, 2, 3], 1, 2);
 * // => [3]
 */
var without = baseRest(function(array, values) {
  return isArrayLikeObject(array)
    ? baseDifference(array, values)
    : [];
});

module.exports = without;

/*
* baseDifference函数解析
* 上面几行都是兼容其他方法调用判断
* 过滤的数组长度是否大于200使用缓存方式去重，优化性能
*/
const LARGE_ARRAY_SIZE = 200
function baseDifference(array, values, iteratee, comparator) {
  let includes = arrayIncludes
  let isCommon = true
  const result = []
  const valuesLength = values.length

  if (!array.length) {
    return result
  }
  if (iteratee) {
    values = map(values, (value) => iteratee(value))
  }
  if (comparator) {
    includes = arrayIncludesWith
    isCommon = false
  }
  else if (values.length >= LARGE_ARRAY_SIZE) {
    includes = cacheHas
    isCommon = false
    values = new SetCache(values)
  }
  outer:
  for (let value of array) {
    const computed = iteratee == null ? value : iteratee(value)

    value = (comparator || value !== 0) ? value : 0
    if (isCommon && computed === computed) {
      let valuesIndex = valuesLength
      while (valuesIndex--) {
        if (values[valuesIndex] === computed) {
          continue outer
        }
      }
      result.push(value)
    }
    else if (!includes(values, computed, comparator)) {
      result.push(value)
    }
  }
  return result
}

```




