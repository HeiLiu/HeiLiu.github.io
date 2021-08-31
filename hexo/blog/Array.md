---
title: 数组
date: 2019-08-5 07:42
tags:
  - Note
  - JavaScript
categories: 前端
---

## 数组  

### 定义：
> 一个存储元素的线性集合， 元素可以通过索引（通常为数字）来任意存取。  

数字索引在内部被转换为字符串类型、这是因为在javaScript中对象的属性名必须是字符串。而数组只是一种特殊的对象

### 创建数组  

  - 通过`构造函数` 
    
    ```js
      <!-- 传入一组元素进行数组初始化 -->
      var arr = New Array(1, 2, 3, 4, 5);
      print(arr.length); // 5
      <!-- 只传一个元素，声明数组的初始化长度, 其中每个元素初始化为 undefined -->
      var arr1 = new Array(10);
      print(arr1.length); // 10
    ```
  <!-- more -->
  - 通过字面量 `[]` 创建  
    ```js
      var arr1 = [];
      var arr2 = [1, 2, 3, 4];
      print(arr2.length); // 4
    ```
  
  - 区别  
  虽然两种创建数组的方法产生的效果一样，但是还是存在一些细微的差别。而且你会发现，在JS开源库中，通过第一种方法来创建数组基本上看不到。
  ```js
  console.time('using[]')
  for(var i=0; i<2000000; i++){var arr = []};
  console.timeEnd('using[]')  // using[]: 29.630859375ms

  console.time('using new')
  for(var i=0; i<2000000; i++){var arr = new Array};
  console.timeEnd('using new')  // using new: 39.93798828125ms
  ```

  通过对比会发现使用new关键字来创建数组花费的时间更长，`实际上`使用new来创建会多一个实例化的过程，在Javascript里分配大量的new变量地址是一项很慢的操作，为了效率起见，你应该始终使用对象符号。虽然`只有在大批量数据的情况下才会有影响`.

  创建一个字符串的方式有：‘字符串’或者是new String('字符串')，一种是直接创建了一个字符串，一个是调用字符串的构造函数创建字符串对象然后再创建这个字符串，中间多了一个创建对象的过程，也许这在一般情况下是看不出什么区别的，但是像上述例子中，当数据足够大的时候，就会发现了问题所在。因此，如果可以通过[]创建一个纯净的数组，就不需要通过new Array()来创建数组。

### 数组方法  

.push() .pop() .shift() .unshift() .indexOf() .find() .findIndex()  .sort() .reverse() .reduce() .fill() .filter() .map() .flat() .splice() .slice() .from() .isArray() .join() .toString() .valueOf()

isArray 

存取函数  
  > 用来访问数组元素的函数，返回目标数组的某种变体  

indexOf() 返回第一个与参数相同的元素的索引, 不存在返回 -1

lastIndexOf 返回相同元素中最后一个元素的索引,不存在返回 -1

数组的字符串表示  

join()

toString()

```js
var names = ["David","Cynthia","Raymond","Clayton","Mike","Jennifer"];
var namestr = names.join();
print(namestr); // David,Cynthia,Raymond,Clayton,Mike,Jennifer
namestr = names.toString();
print(namestr); // David,Cynthia,Raymond,Clayton,Mike,Jennifer
```
由已有数组创建新的数组  

- concat()  合并多个数组创建一个新数组  
- splice()  方法通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。此方法会改变原数组  
- slice()   方法返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）[begin, end)。原始数组不会被改变

使用 splice() 方法为数组添加元素，需提供如下参数：  
• 起始索引（也就是你希望开始添加元素的地方）；  
• 需要删除的元素个数（`添加元素时该参数设为 0`）；  
• 想要添加进数组的元素。  
```js
var nums = [1,2,3,7,8,9];
var newElements = [4,5,6];
nums.splice(3,0,newElements);
print(nums); // 1,2,3,4,5,6,7,8,9
```

可变函数  
> 能够改变数组的内容的函数  

push() 在末尾添加元素
pop() 删除末尾的元素

shift()
unshift() 添加元素到数组的开头

reverse() 翻转数组
sort()  数组排序
对数组进行排序是经常会遇到的需求，如果元素是字符串类型，那么sort() 就非常好使：  
```js
var names = ["David","Mike","Cynthia","Clayton","Bryan","Raymond"];
names.sort();
print(names); // Bryan,Clayton,Cynthia,David,Mike,Raymond
```

但是如果数组元素是数字类型，sort() 方法的排序结果就不能让人满意了：
```js
var nums = [3,1,2,100,4,200];nums.sort();print(nums); // 1,100,2,200,3,4sort() 
```
方法是按照`字典顺序`对元素进行排序的，因此它 `假定元素都是字符串类型`，在上一个例子中，即使元素是数字类型，也被认为是字符串类型。为了让 sort() 方法也能排序数字类型的元素，可以在调用方法时传入一个大小比较函数，排序时，sort() 方法将会根据该函数比较数组中两个元素的大小，从而决定整个数组的顺序。  
`对于数字类型，该函数可以是一个简单的相减操作，从一个数字中减去另外一个数字。如果结果为负，那么被减数小于减数；如果结果为 0，那么被减数与减数相等；如果结果为正，那么被减数大于减数。`
```js
function compare(num1, num2) {
  return num1 - num2;
}
var nums = [3,1,2,100,4,200];
nums.sort(compare);print(nums); // 1,2,3,4,100,200
```
sort() 函数使用了 compare() 函数对数组按照数字大小进行排序，而不是按照字典顺序。

迭代器方法  

其实可以多展开来讲的 map foreach filter reduce 的区别

- forEach()  
- every()  
- some()  
- map()  
- filter()  
