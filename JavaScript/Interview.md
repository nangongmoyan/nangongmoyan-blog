# JavaScript面试题

## 基础知识

### 说说var、let、const之间的区别
1.在变量提升上，var声明的变了存在变量提升（变量可以在声明之前调用，值为undefined），let和const不存在变量提升，即它们所声明的变量一定要在声明后使用，否则报错；

2.在暂时性死区上，var不存在暂时性死区，let和const存在暂时性死区，只有等到声明变量的那一行代码出现，才可以获取和使用该变量

3.在块级作用域上，let和const存在块级作用域，var不存在；

4.在重复声明变量上，var允许重复声明变量，在声明多个相同的变量时，后面的变量会覆盖前面的变量，let和const在同一作用域下不支持重复声明；

5.在修改变量上，var和let支持修改，const因为是声明的是常量，一旦声明，常量的值就不能改变

此外，const是用来声明常量，且不能改动，所以用const声明的常量需要初始化；const实际上保证的并不是变量的值不得改动，而是变量所指向的那个内存地址所保存的数据不能改动，对于简单数据类型，值就保存在变量所指向的那个内存地址，因此等同于常量，对于复杂类型，变量所指向的那个内存地址所保存的是一个指向实际数据的指针，const只能保证这个指针是固定的，不能保证所指向的那个实际数据的结构不会改变。

### null和undefined的区别
null表示一个“无”的对象，也就是该处不应该有值；而undefined表示未定义

在转换数字时结果不同，Number(null)为0， Number(undefined)为NaN

在使用场景上，对于null来说，可以作为对象原型链的终点；当它作为函数的参数，表示该函数的参数不是对象。对于undefined来说，变量被声明了，但没有赋值时，就等于undefined；调用函数时，应该提供的参数没有提供，该参数等于undefine；对象没有赋值属性，该属性的值为undefined；函数没有返回值时，默认返回undefined

### typeof和instanceof的区别

typeof表示对某个变量类型的检测，基本数据类型中除了null会返回object外，都能正确的显示为对应的类型；引用数据类型中除了函数护理显示function，其它都显示为object

instanceof主要是用于检测某个构造函数的原型对象在不在某个对象的原型链上，返回布尔值

```
function myInstanceof(left,right){
    /** */
    let proto = Object.getPrototypeOf(left)
    
    while(true){
        /** 到原型链末尾还没找到*/
        if(proto === null) return false
        /** 找到了*/
        if(proto === right.prototype) return true
        
        /** 获取原型链上下个值重新赋值*/
        proto = Object。getPrototypeOf(proto)
    }
}
```

备注：typeof对于null错误的显示为object是JavaScript的一个悠久bug，在JavaScript最初的版本中使用的是32位系统，为了性能考虑使用低位存储变量的类型信息，000开头代表的是对象，而null正好表示为全零，所以将它错误的判断位object

### 一句话描述一下this

对于函数而言指向最后调用函数的那个对象，是函数运行时内部自动生成的一个内部对象，只能在函数内部使用

对于全局来说，this指向window

### 描述一下EventLoop的执行过程

1.一开始将整个脚本作为一个宏任务执行

2.执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务添进入微任务队列

3.当前宏任务执行完出队，检查微任务队列，有则依次执行，直到全部执行完

4.执行浏览器UI线程的渲染工作

5.检查是否有Web Work任务，有则执行

6.执行完本轮的宏任务，回到2，依此循环，直到宏任务和微任务队列都为空


## ES6+

### 你是怎么理解ES6新增Set、Map两种数据结构的

Set是一种叫集合（是由一堆无序的、相关联的且不重复的内存结构组成的组合）的数据结构，Set实例的方法：(增删改查)add、has、delete、clear 、（遍历）keys、values、entries、forEach

Map是一种叫字典（是一些元素的结合，每个元素都有一个称作key的域，不同元素的key各不相同）的数据结构；Map实例的方法：(增删改查)set、get、has、delete、clear、（遍历）keys、values、entries、forEach

WeakSet接受一个Iterable接口的对象作为参数，它没有Set遍历相关的API，没有size属性，它的成员只能是引用类型，不能是其他类型的，WeakSet里面的引用只要在外部消失，它在WeakSet里面的引用就会自动消失

WeakMap结构和Map结构类似，也是用于生成键值对的集合，它没有Map遍历相关的API，没有clear清空方法，它只接受对象作为键名（null除外），不接受其他类型的值作为键名，WeakMap的键名所指向的对象，一旦不再需要，里面的键名对象和所对应的键值对会自动删除引用，注意WeakMap弱引用的只是键名，而不是键值，键值依然是正常引用

### 描述一下Promise

Promise是一个对象，它代表了一个异步操作的最终完成或失败，由于它的.then、.catch、.finally方法返回的是一个新的Promise对象，所以允许我们链式调用，解决了传统回调地狱的问题

1.Promise的状态一旦改变就不能更改了，也就是状态从pedding变为resolved后，就不能从resolved变回pending，或从resolved变为rejectd

2..then和.catch中不能返回promise本身，否则会造成死循环

3..finally不管Promise对象最后的状态为resolved还是rejected，都会执行里面的回调函数

4..finally方法中是不接收任何参数，所以无法知道Promise的状态是resolved还是reject

5..then和.catch的参数期望是函数，如果不是函数则会发生值透传

6.在Promise中，返回任意一个非Promise的值都会被包裹成Promise对象，如return 2会被包装成return Promise.resolved(2)

7..catch不管被连接到哪里，都能捕获上一层未捕捉过的错误

8..then和.catch里面return 一个error对象并不会抛出错误，也不会被后续的catch中捕获

9..finally最终返回的默认是一个上一次Promise对象值，如果抛出一个异常则返回异常的Promise对象

10..then和.catch可以被调用多次，但如果Promise内部的状态一经改变，并且有了一个值，那么后续每次调用.then和.catch的时候都会直接拿到该值

11..then方法是能接收两个参数的，第一个是处理成功的函数，第二个是处理失败的函数，在某些时候可以认为.catch 是.then第二个参数的简便写法

12.all的作用是接收一组异步任务，然后并行执行异步任务，并且在所有异步操作执行完后才执行回调

13.race的作用是接收一组异步任务，然后并行执行异步任务，只保留第一个执行完成的异步操作的结果，其他的方法仍在执行，不过执行结果会被抛弃

14.Promise.all().then()结果中数组的顺序和Promise.all（）接收到的数组顺序一致

### Promise.all和Promise.race中如果有一个抛出异常了会如何处理

all和race传入的数组中如果抛出异常的一步任务，那么只有最先抛出的错误会被捕获，并且是被then的第二个参数或者后面的catch捕获，但不会影响数组中其他异步任务的执行

### Promise为什么能链式调用

因为Promise的.then、.catch、.finally方法返回的是一个新的Promise对象

### Promise.all()是并发的还是串行的

是并发的，不过Promise.all().then()结果中数组的顺序和Promise.all（）接收到的数组顺序一致
