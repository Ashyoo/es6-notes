# es6-notes

## let

* ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。
* let 不存在变量提升,var 存在变量提升，也就是在变量声明之前使用，输出undefined

```javascript
    // var 的情况
    console.log(foo); // 输出undefined
    var foo = 2;

    // let 的情况
    console.log(bar); // 报错ReferenceError
    let bar = 2;
```

* 暂时性死区(也就是变量的提升)

```javascript
    var tmp = 123; 

    if (true) {
      tmp = 'abc'; // 报错ReferenceError
      let tmp;
    }
```

上面代码中，存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错

* 不允许重复声明 let不允许在相同作用域内，重复声明同一个变量。

```javascript
    // 报错
    function func() {
      let a = 10;
      var a = 1;
    }

    // 报错
    function func() {
      let a = 10;
      let a = 1;
    }
```


## const

* 声明一个只读的常量。一旦声明，常量的值就不能改变。
* const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

```javascript
    const foo;
    // SyntaxError: Missing initializer in const declaration
    对于const来说，只声明不赋值，就会报错。
```

* const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用

```javascript
    if (true) {
      const MAX = 5;
    }
```

* const声明的常量，也与let一样不可重复声明。

* **const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动，变量的引用是可以改变的。**

## 箭头函数

* **函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。**

> this对象的指向是可变的，但是在箭头函数中，它是固定的。

```javascript
    function foo() {
      setTimeout(() => {
        console.log('id:', this.id);
      }, 100);
    }
    var id = 21;
    foo.call({ id: 42 });
    // id: 42
```

* 注意：箭头函数里面根本没有自己的this，而是引用外层的this。

* 如果箭头函数的函数体中,只有一个表达式(值),那么{}和return这个关键字是可以省略掉的

* 如果箭头函数只有一个参数,参数列表中的括号是可以省略掉的

## 取json对象的值的两种方法

```javascript
    let data = {
    	name: '111',
    	'2': '222',
    	'3': '333'
    }
    alert(data.name) // 111
    alert(data['2']) // 222
```

## es6解构赋值

* 数组的解构赋值

```javascript
    let [a,c] = new Set([1,2]) // Set 定义一个集合，里面的值不能重复
    console.log(a,c)

    let [a1, b = 'default'] = ['3']
    console.log(a1,b)
```

* 对象的解构赋值

```javascript
    import { Select, Input, DatePicker, Table } from 'antd'
    // 会从antd中分别取到相对应的变量
```

## 数组

* 使用扩展运算符 ... copy数组或者对象

```javascript
    const arrs = [1,2,3]
    const itemArrs = [...arrs]
```

* 使用Array.from方法，将类似数组的对象转为数组

```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
```

* 数组的操作

```javascript
    const shopId = 'qwerafdhdhkghjgk'
    let shops = [{
      'name': '今日头条2',
      'info': 'qwerafdhdhkghjgk',
      'generaDate': '昨天3',
      'wtf': 'what fuck',
      'content': 'See you tomorrow'
    }]
    let look = shops.filter(item => shopId.includes(item.info))
        // filter 数据的过滤 指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组
        // includes 表示是否能找到匹配参数字符串，返回true和false
      .map(temp => temp.wtf)
        // map 用来遍历数组
      .join('')
        // 用来将数组转换成字符串
```

* es6的继承

```javascript
    class Your {
        constructor(){
            this.type = 'you'
        }
        says(say){
            document.write(`${this.type}say${say}`)
        }
    }

    let your = new Your()
    your.says('hello') // you say hello

    class My extends Your {
        constructor(){
            super()
            this.type = 'I'
        }
    }

    let my = new My()
    my.says('hi') //I says hi
```

上面代码首先用class定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。简单地说，constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实力对象可以共享的。

<br>

Class之间可以通过extends关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。上面定义了一个My类，该类通过extends关键字，继承了Your类的所有属性和方法。

<br>

**super关键字，它指代父类的实例（即父类的this对象）。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。**

## promise 用来传递异步操作

简单说，它的思想是，每一个异步任务返回一个Promise对象<br />

**该对象有一个then方法和catch方法：**

* then() 可以接收两个参数,第一个对应Promise中调用的resolve,第二个对应reject
* catch() 和then的第二个参数一样,可以接收reject的回调,如果then方法的第一个参数中抛出错误时,也会进到catch方法中

```javascript
    // 用setTimeout来模仿异步操作,和ajax是一样的意思

    var fn1 = sum => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                if (sum) {
                    resolve(sum.split('').reverse('').join(''))
                } else {
                    reject('这里是一个❌')
                }
            }, 2000)
            
        })
    }

    fn1('abcde')
    .then(resolve => console.log(resolve)) // edcba

    fn1()
    .then(resolve => console.log(resolve))
    .catch(reject => console.log('进入了catch的reject')) // 进入了catch的reject

    fn1()
    .then(resolve => console.log(resolve), reject => console.log('进入了then的reject'))
    .catch(reject => console.log('进入了catch的reject')) // 进入了then的reject
   
    // *-----------------------再来一种--------------------------------* //

    var fn2 = () => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('大概我是咸鱼吧')
            }, 2000)
        })
    }

    // 在resolve中抛出错误
    fn2()
    .then(resolve => { throw Error }, )
    .catch(reject => console.log('收到错误')) // 收到错误

    // 重新试一种情况
    fn2()
    .then(resolve => { throw Error }, )
    .then(resolve => {}, reject => console.log('进入then中的reject'))
    .catch(reject => console.log('收到错误'))
    // 进入then中的reject

    // 大概是这样吧,在then方法中的reject回调中接收了错误❎,便不再会继续执行catch了
```
