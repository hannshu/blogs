---
categories: ts_tutorial
title: TypeScript 简介
---

# 安装typescript  
- 安装node.js  
node.js是一个javascript的运行环境  
对于Ubuntu系统，可以直接通过[这个网站](https://nodejs.org/en/download/package-manager/)的指令直接下载和安装  
``` shell
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

- 安装typescript  
可以直接通过以下命令全局安装  
``` shell
sudo npm i -g typescript
```

对于大多数浏览器都不支持直接读取typescript，所以需要通过typescript compiler将`.ts`文件转换为浏览器能正常使用的`.js`的javascript文件。  
需要现在项目下初始化`tsc`，生成`tsconfig.json`配置文件，之后只需要在控制台输入`tsc`即可转译目录下的所有`.ts`文件。  
``` shell
tsc --init
tsc
tsc -w  // 可以用来监听目录下的文件变化
```  

## 变量声明  
在声明变量时需要指明变量的类型(类型如果不自己定义，则ts会自动定义类型)，如  
``` ts
var test: string = '测试';
```
- 注:  
1. 类型的首字母应该小写。  
2. 在后面的语句中，不能给这个变量赋其他类型的值，否则会报错。  

# 断言与联合  
对于未知的变量，可以使用断言强制变为某种类型  
同样，可以使用联合(`|`)来定义类型  
``` ts
// 断言
var button = document.querySelector('button') as HTMLButtonElement;
// 联合
var button: HTMLButtonElement | null = document.querySelector('button');
```  
-> 在定义任何东西时应该定义类型，在调用任何东西时应该检测类型  

# 接口与实现
对于类，也需要声明其属性的类型，这时就可以使用接口来实现类  
如果一个接口需要实现多个类，其中某些变量不是所有类通用，即可以通过`?`来设置为非必需  
``` ts
interface CatType {
    name: string;
    gender: boolean;
    age: number;
    birth?: string; // 不一定含有这个参数
}

class Cat implements CatType {

    name: string;
    gender: boolean;
    age: number;

    constructor(name: string, gender: boolean, age: number) {
        this.name = name;
        this.gender = gender;
        this.age = age;
    }
}
```

# 修饰符和函数类型  
函数的四种修饰符`static`, `public`, `private`, `protected`。
| 修饰符       | 作用                              |
|-----------|---------------------------------|
| static    | 对象不可调用，只能通过类或其子类直接调用            |
| public    | 在任何地方都可以访问                      |
| private   | 只能在类内部使用，不能通过对象调用，也不能被子类使用      |
| protected | 可以被类内的非static方法访问，不能通过对象或是类直接访问 |
对于一个函数，需要设定其返回值的类型，如果不存在返回值，则定义为`void`  
``` ts
class Display {
    static showData(data: DataType): void {
        ...
    }
}
```
对于可能为`null`的变量，可以通过在变量后加`?`来解决需要判定是否为空的问题(如果为空，则不执行该操作)  

# 范型和异常
范型：使用一个*类型变量*来表示一种类型，类型值通常是在使用的时候才会设置。  
就如下面的这个函数，我们在编写函数时，根据`fetch()`返回值的不同，得到的`id`的结果也会有所不同，但是一定是一个`Promise`类的结果，所以，我们只需要在调用时再标注出结果即可。这里的`T`只是表示类型的一个变量。  
``` ts
function getID<T>(data: DataType): T {
    var id: T = get_id_from_url<T>(data.url);
    return id;
}
var id = getID<string>(data);   // 我们要得到的id是`string`类型的
```
异常：`ts`通过`try-catch`来进行异常处理  
``` ts
try {
    ...
} catch(error: Error | unknown) {
    let message: string;
    if (error instanceof Error) {
        message = error.message;
    } else {
        message = String(error);
    }
    console.log(error);
}
```

# 同步和异步  
同步任务：这些任务在主线程上按顺序排队执行。  
异步任务：这些任务不直接在主线程上执行，而是被分配到其他线程执行，等到主线程依次将同步任务执行完毕之后，才会进入主线程执行。  
- 异步执行机制:  
1. 所有同步任务都在主线程上执行，形成一个执行栈  
2. 主线程之外，还存在一个"任务队列"。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件  
3. 一旦"执行栈"中的所有同步任务执行完毕，主线程就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。  
4. 主线程不断重复上面的第三步。  

## js的Promise机制  
Promise会将任务封装为一个Promise类的对象，这个对象会将任务自动运行并得到任务结果，而且在得到结果的过程中并不会影响到其他任务的进行。由此实现多个任务的并发进行。  
实现异步的过程被隐藏在Promise类的实现过程中，我们只需要将任务交给Promise，Promise则会返回一个对象，之后通过这个对象去拿任务结果就可以了。  

``` js
function AsyncFunc1(args) {
    function task(resolve, reject) {
        ...
    }
    return new Promise(task);   // 将task交给Promise
}
promise = AsyncFunc1(args); // 得到一个对象，通过这个对象来获取task的结果
promise.then(
    function(value) {
        ... // task执行成功，对结果进行处理
    },
    function(error) {
        ... // task执行失败，对结果进行处理
    }
);
```

# 事件监听  
比如监听对于某个按钮的点击事件  
``` ts
button?.addEventListener<'click'>('click', resolve_function);   // 这里最好标注范型，可以防止写错事件名称
```

