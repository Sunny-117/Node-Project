# 原生 JS

# 设计模式

Square->SnakeHead->snakeHead

## 继承：

方法 1：圣杯模式

```javascript
var tool = {
  inherit(target, origin) {
    //继承    target:目标对象 origin：源对象
    var F = function () {}; //空函数，中间层
    F.prototype = origin.prototype;
    target.prototype = new F();
    target.prototype.constructor = target; //修正一下继承者的构造函数
  },
};
// Square => SnakeHead => snakeHead

function Square(x, y, width, height) {
  this.x = x;
  this.y = y;
  this.width = width;
  this.height = height;
}
Square.prototype.collide = function () {
  console.log("collide");
};

function Food() {
  //food继承square
}

tool.inherit(Food, Square);
var f = new Food();
f.collide(); // 继承成功
```

> 太麻烦

方法 2：extend：也继承私有属性

```javascript
var tool = {
  inherit(target, origin) {
    //继承    target:目标对象 origin：源对象
    var F = function () {}; //空函数，中间层
    F.prototype = origin.prototype;
    target.prototype = new F();
    target.prototype.constructor = target; //修正一下继承者的构造函数
  },
  extends(origin) {
    //扩展
    var target = function () {
      //私有属性的继承：传参可以接受到
      origin.apply(this, arguments); //this==target
    };
    this.inherit(target, origin); //必须有继承

    return target; //必须有返回值，否则为undefined
  },
};
Square.prototype.collide = function () {
  console.log("collide");
};
var Food = tool.extends(Square);
var f = new Food(10, 10, 100, 100);
console.log(f.x); //10
f.collide();
```

方法 3：single 闭包 单例模式
闭包：不会被垃圾回收机制回收

```javascript
function a() {
  var n = 10;
  return function b() {
    n++;
    console.log(n);
  };
}
var fn = a();
console.log(fn()); //11
console.log(fn()); //12
console.log(fn()); //13
console.log(fn()); //14
```

# 单例模式

```javascript
var tool = {
  inherit(target, origin) {
    //继承    target:目标对象 origin：源对象
    var F = function () {}; //空函数，中间层
    F.prototype = origin.prototype;
    target.prototype = new F();
    target.prototype.constructor = target; //修正一下继承者的构造函数
  },
  extends(origin) {
    //扩展
    var target = function () {
      //私有属性的继承：传参可以接受到
      origin.apply(this, arguments); //this==target
    };
    this.inherit(target, origin); //必须有继承
    return target; //必须有返回值，否则为undefined
  },

  single(origin) {
    //单例
    var target = (function () {
      var instance; // undefined
      return function () {
        if (typeof instance == "object") {
          //有没有创建过
          return instance;
        }
        origin.apply(this, arguments);
        instance = this; //this是target
      };
    })();

    this.inherit(target, origin);

    return target;
  },
};

function Square(x, y, width, height) {
  this.x = x;
  this.y = y;
  this.width = width;
  this.height = height;
}
Square.prototype.collide = function () {
  console.log("collide");
};
var Food = tool.single(Square);
var f1 = new Food(10, 10, 100, 100);
var f2 = new Food(20, 20, 200, 200);
console.log(f1 == f2); //true
console.log(f1, f2); //10
```

# 工厂模式

move 链表内部原理

![image-20211205112958502](/Users/sunny/Library/Application Support/typora-user-images/image-20211205112958502.png)
