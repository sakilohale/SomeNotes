# Note

## 2020/09/25

复习了

- 基本数据类型各自的区别
- 如何并在何时使用typeof，instanceof
- NaN类型 和 isNaN函数
- 转型函数Number，parseInt，parseFloat
- object类型（object实例的原型属性和方法）
- object对象构造器的方法及对象属性的特性（数据属性，访问器属性）
- 对象的创建（工厂模式，构造函数模式（及其优化-----原型模式））
- 对应test1.js

```javascript
//练习代码如下：
// typeof用法
 var un;
 var nu = null;
 var num = 1;
 var st = '123'
 var bo = true;
 var ob = {}

 /* 推荐显式声明并初始化变量，这样在后面可以利用typeof来检测未声明变量了 */
 console.log(typeof unn) //undefined (未声明)
 console.log(typeof un) //undefined (声明未初始化则为undefined)

 /* 意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null值 */
 console.log(typeof nu) //object （代指空对象引用）

 console.log(typeof num) //number
 console.log(typeof st) //string


 console.log(typeof bo) //boolean

 console.log(typeof ob) // object

 console.log( NaN == NaN) //false NaN与任何数值都不相同，包括自身
 console.log(isNaN('string')) //true 因为英文字符串不能转换成数值
 console.log(isNaN('12')) // false 可以转换成数值
 console.log(isNaN(true)) // false 可以转换成1



 /* --------------------分割线--------------------*/





//非数值转换为数值
 // Number()转型函数, parseInt(), parseFloat().
 // 第一个可用于所有数据类型，后两个则专门将字符串转换为数值(整数和浮点数)。


 console.log(Number(true)) //1
 console.log(Number(11))//11
 console.log(Number(null))//0
 console.log(Number(undefined))//NaN
 console.log(Number('111'))//111
 console.log(Number('0111'))//11
 console.log(Number('11.1'))//11.1
 console.log(Number(''))//0
 console.log(Number('111a'))//NaN
 console.log(Number(ob)) // 先valueof()，如果转换成了非数值NaN，则调用对象的toString()方法，再按照以上规则返回




 console.log(parseInt('11'))//11
 console.log(parseInt('070'))// ES3中开头为0且后跟数字则解析为八进制，ES5后引擎不在解析。所有这里为70。
 console.log(parseInt('070',8))//56 接受第二个参数，用于判断进制
 console.log(parseInt('0xf'))//15 0x开头的解析为十六进制
 console.log(parseInt('1234blue'))//1234 只输出数值
 console.log(parseInt('123.4'))//123 同理，但没有四舍五入
 console.log(parseInt(' 1234'))//1234
 console.log(parseInt('  1234'))//1234 由此可知这个函数会忽略字符串前面的空格直至找到第一个数值
 console.log(parseInt('a123'))//NaN 第一个字符为非数值或符号时则返回NaN


console.log(parseFloat('22.5.4')) //22.5  和parseInt实际上是一样的用法，只是会保留第一个小数点后的数值


 /* --------------------分割线--------------------*/


 st = '132456' //st 为字符串类型， 这里并不是赋值更改，而是销毁了原先的字符串，然后再用新的字符串填充这个空的变量

 /*  字符串转换函数toString()，几乎每个值类型都有（不包括null和undefined） ，但是转型函数String()都可  */


 /* --------------------分割线--------------------*/


 var o = new Object();// 讲讲Object类型。 Object类型是所有对象实例的基础（原型）
 function o1() {
this.name = 'ljy'
 }
 o1.prototype = o
 var o1 = new o1()

var o2 = {
     _year:10,
     edition:1,
}


 /* 它的实例方法主要如下 */

 console.log(o.constructor) // constructor 属性用于保存着创建该对象的函数，在这里构造函数便是Objeci();
 console.log(o1.hasOwnProperty('name')) // （true） hasOwnProperty(Propertyname) 用于判断该属性是否再该对象实例中存在（而非原型），其中Propertyname必须为字符串
 console.log(o.hasOwnProperty('name'))  // false
 console.log(o.isPrototypeOf(o1)) //  true  用于判断该对象是否为传入对象的原型
 console.log(o1.propertyIsEnumerable('name')) // 用于判断传入的对象是否能够用for-in枚举
 console.log(o1.toLocaleString())// 和toString的区别，该字符串与执行环境的地区对应
 console.log(o1.toString())
 console.log(o1.valueOf())


 /*  接下来是一些Object构造器对象的方法，主要用于操作数据属性，访问器属性 */
/*   Object.defineProperty    */

Object.defineProperty(o1,'name',{
    value:'LJY',
    writable:true,
    enumerable:true,
    configurable:true,
})     //  四大数据属性：value（值）, writable（能否写改）, enumerable（能否for-in）, configurable（能否delete）, 其实也可以看作是对象属性的属性，也就是数据属性，主要用途用来保护数据。
       //  默认不修改的话这四个属性除了value为undefined其余都为true。 其中configurable一旦改为false则不可逆了



//  定义访问器属性的方法。
Object.defineProperty(o2,'year',{
    get() {
        return this._year;
    },
    set(newValue) {
        if(newValue == 2){
            this._year=2;
            this.edition=2;
        }
    }
})

 o2.year = 2
 console.log(o2)//会发现_year和edition都变成2了

 /* 这样看来定义访问器属性主要是用来自定义逻辑来保护或者改变其他属性 */


/* 其实访问器属性和数据属性可以一起设置， 用的是Object.defineProperties()方法*/
Object.defineProperties(o2,{
    _year:{value:50,writable:true},
    edition:{value:15,writable:true},
    year1:{
        get() {
            return this._year;
        },
        set(newValue) {
            if(newValue > this._year){
                this.edition = 0
                this._year = newValue
            }
        }
    }
})

 o2.year1 = 51    //这里设置的访问器属性为year1是因为访问器属性不支持redefined，注意，当writable属性为false的时候改值也没用哦
 console.log(o2) //看看新的配置的o2对象，会发现_year = 51 ,edition = 0

 /* 若是想要查看对象某个属性的特性 用Object.getOwnPropertyDescriptor()，会返回一个对象*/
 console.log(Object.getOwnPropertyDescriptor(o2,'_year'))



/* 上面介绍完了对象的属性特性，接下来便是开始创建对象 */

/* 工厂模式 */
function createf() {
var oo = new Object();
oo.name = '123';
oo.sayname = function () {
return  oo.name
}
return oo;
}
var oo1 = createf();


/* 构造函数模式 */
 function createF() {
  this.name = '1234';
  this.sayname=function () {
    return this.name
  }
 }

var oo2 = new createF();

 /* 识别对象类型有两个办法，第一个就是用object类型的原型实例属性construct，另一个就是instanceof */

 console.log(oo2.constructor); // function createF()
 console.log(oo2 instanceof createF); //true


 /*  工厂模式和构造函数模式相比起来的话，缺少了对象类型识别的功能，所以建议构造函数模式。*/
 /* 构造函数模式的核心是函数里面的this对象，和构造时用到的new */
 /* 其中new干了四件事情，1.创建一个新对象；2.将构造函数的作用域赋给新对象（也就是让this指向这个对象；3.执行构造函数中的代码；4.返回新对象 */
/* 上文中的构造函数还能优化，因为sayname这个属性函数在每次实例化一个createF对象时都会重新实例化一个Function实例，可以通过全局函数方法，但不利于封装，所以建议用原型模式来解决。 */

 /*改写*/

 function createF2() {
this.name = '12345'
 }

 createF2.prototype.sayname = function () {
return this.name;
 }

var oo3 = new createF2();

 console.log(oo3.sayname());

 /*  */
```





## 2020/09/26



主要看了原型的由来，原型链，构造函数，对象的创建，继承。

- 如何创建一个对象的原型prototype对象
- 遍历对象属性的方法，获得对象原型的方法，判断是否为该对象原型的方法。（也就是各种Object原型的方法）
- 原生对象Array，String等的原生方法，和自定义原型方法
- 创建一个对象的最优方法（构造函数模式 + 原型模式）
- 让一个对象继承另一个对象的最优方法（借用构造函数 + 原型模式）
- 对应test2.js

```javascript
//   2020/9/26


// 继续对象的一个延申，讲到了原型

// 原型 prototype 本质是一个对象实例，将它赋给其他构造函数后便成为了该构造函数对象的原型，那么这个由构造函数创造的对象都拥有原型对象的实例属性和方法，且都指向的同一个哦。

// 如何创造原型对象 如下所示

// 创建好原型对象
var ob1 = {
    name:'ljy',
    age:'20',
    sayname:function () {
     return this.name + '————' + this.age;
    }
}

// 构造函数
function ob() {
this.sex = 'man'
}

// 将ob1作为原型并创建对象
/* '在这里根据原型的动态性，先创建对象再设置原型也是ok的' 书上如是说，但是fb不允许，目前不知道为啥 */
ob.prototype = ob1;
var ob2 = new ob();




//在对象中查看他的prototype，没错，就是ob1
console.log(ob2)
//更直接一点则是
console.log(ob.prototype)
//反过来在ob1中找ob（）也是可以的，在constructor属性中
console.log(ob.prototype.constructor)//ob1.constructor


console.log(ob.prototype.isPrototypeOf(ob2)) //true

console.log(Object.getPrototypeOf(ob2) == ob.prototype)//true，这说明了对象实例也有一个指向原型对象的指针（假设为[Prototype]）

//先遍历对象实例属性或方法，没有的话再去上一层原型去找
console.log(ob2.name) // ljy
console.log(ob2.sayname()) // ljy————20


// 那么，如何确认这个属性是在对象实例上的还是在原型对象上的诶
// 可以用 in操作符 加 hasOwnProperty（）方法

//例如判断ob2.name是否存在且，是实例的还是原型的

console.log(ob2.hasOwnProperty('name'))  //false 这个方法只遍历对象实例上的属性
console.log('name' in ob2) //true , in 操作符 无论该属性是在实例或者原型，只要存在都会返回true

// 遍历对象的属性，就会想到，常用的遍历方法for-in，for-in遍历对象属性时，会遍历该对象包括原型的所有的可枚举（enumerable==true）的属性
// 但有个bug，就是当对象的属性或方法与原型中不可枚举的对象或方法同名的话，会被误以为是在重新实现，因为不可枚举所以会出错。

for (item in ob2){
    console.log(item)
}// sex,name,age,sayname  都是字符串形式


// 除了这个形式还有 Object.getOwnPropertyNames（）
// 这个只能返回对象实例上的属性和方法，原型不行。
console.log(Object.getOwnPropertyNames(ob2)); //返回字符串数组Array['sex'];

//还有Object.keys();参数为对象,作用是返回这个对象的属性和方法，也不能返回原型的。
console.log(Object.keys(ob2)) // 返回字符串数组 Array['sex']
console.log(Object.keys(ob.prototype))// 返回字符串数组 Array['name','age','sayname']


//以上都是自定义的一些对象，那么原生对象Array，String，Objece这些对象的原型又是怎样的呢？
//其实，这些原生对象的方法都来自其原型

console.log(Array.prototype)
console.log(String.prototype) //可以发现Array和String的原型的原型是Object.prototype

console.log(Object.prototype)
console.log(Object.prototype === Object.getPrototypeOf(Array.prototype))  //true!!!

// 自定义原型方法，这个很管用。但是啊，一旦重写了哪个原型的已有方法那就悲剧了。 建议先看看原型中有哪些方法和属性，避免重名。
// 这里自定一个startwith方法，实现和其原型方法startsWith一样的作用。
String.prototype.startwith = function (text) {
    return this.indexOf(text) == 0;
}

var st = 'hello world';
console.log(st.startwith('hello')); //true



//所以这里能够看出啦！！！ 原型模式 + 构造模式 = 完全体 = 自定义原生对象

// 原理则是： 特有属性放再对象实例中，方法和共同属性放在原型中。 其实之前已经写过了，这里就不写了。

//如何写一个原型对象 ， 有两个方式。
// 1. 像之前一样，创建一个对象，然后p.prototype = 对象
// 2. p.prototype = {
//    constructor:p,
//     属性和方法。
// }



// 前面讲了这么多啊，其实就是为了继承啊。

// 继承有2种基础方式，但是都有各自的缺陷，接下来一一介绍一下。

// 原型链继承

// 顾名思义，就是把继承看作是原型链的连接。

function superType() {
this.name = 'ljy';
};
function subType () {
    this.age='20',
    this.sayAge=function () {
         return this.age
    }
};
//这里，想让 super 继承 sub 的属性和方法，原理就是让super的原型为subType就行了撒。
superType.prototype = new subType();
var supertype = new superType();
console.log(supertype.age+' __ '+supertype.sayAge()) // 20

// 但是啊，致命缺点啊就是，所有superType.constructor（也就是superType实例的构造函数）的实例，他们的age属性都是共享的，也就是他们没有属于自己的age！
// 同样，方法也是共享的，但这起到了函数复用的效果，是优点。


//接下来，第二个方法是借用构造函数（经典继承）

//原理是在需要继承的构造函数内引用被继承的构造函数，执行环境为该需要继承的构造函数。

function superType1() {
    this.name = 'ljy';
    subType1.apply(this,20);
};
function subType1 (age) {
    this.age = age,
        this.sayAge=function () {
            return this.age
        }
};

var supertype1 = new superType1()

console.log(supertype1) // 你会发现 ， 哦， 全在里面去了！！nice！！
// 在这里subTyoe1()被称为超类型函数，superType1（）则是子类型函数。 这个模式则是子类型继承超类型
// 但是啊，还记得构造函数模式时说到的弊端嘛。 方法都在函数中，每次创建一个实例都会实例化对应方法个数的Function实例，影响效能。
// 那你说，把这些方法放在超类型的原型中不就好了嘛
// 想得美啊，这个模式中子类型无法调用超类型函数原型的方法。
// 所以啊，原型链我又回来了。


// 最佳继承： 借用 + 原型链
// 借用属性，原型上写方法。

//  我的想法就是， 平常创建对象的最佳方法为：写构造函数，属性写里面，方法写原型里。 然后继承时就会比较方便，因为构造函数里面没得方法，所以可以直接借用。
//  然后再去它的原型里找方法，赋给自己的原型就行。（指向同一个原型？）
```



## 2020/09/27 — 28

看了引用类型，主要是一些对象的使用。

- 对象的点表达式和方括号表达式

- Array类型解析

- Array原型方法使用

- 对应test3.js

  ```javascript
  // 2020/09/27
    // 对象讲了这么多，也聊到了Object，Array等等
   //  回想一下，基本数据类型有：null，undefined，string，boolean，number。他们有自己的特性，也有自己的转型函数。
   //  相对比较复杂的。引用类型，今天来瞅瞅。
  
   //引用类型有Object，Array，Date，RegExp，Function
   //其实呀，引用类型不过是一个称呼。因为引用类型的值（实例），其实就是对象。那么这个引用类型其实就可以看作构造函数啦。
   //可以验证一下
  
   console.log(Array);//返回了一个函数
   console.log(new Array());//返回了一个Array数组，new其实可以省略。
  
  
   // 那么，其实昨天是学习的对象的本质，今天来学习如何使用对象
  
   //引用对象属性，有两个方法
  
   function ob() {
      this.name = 'ljy',
      this.age = '20'
   };
  
  
   ob.prototype.intro = function () {
    return this.name + '__is__' + this.age
   };
  
   var ob1 = new ob();
  
   //1.点表示法，相对稳定直接，但是属性名为关键字或者保留字，或者有空格等字符时用不了。再用变量表示属性的时候也不适用。
  
   console.log(ob1.name)//ljy
  
  
   //2.方括号表示法，属性名必须为字符串形式，可以解决点表示法的所有缺点，但容易和Array类型搞混。。。字符串也容易忘记。
   //并且创建对象的时候，this.方式创建的属性也不允许什么空格神秘符号之类的，总之有点不搭
  
   console.log(ob1['name'])//ljy
  
   //可以这样用
  
   var ooo = {};
   ooo['First name'] = 'liu';
   ooo['My age'] = '20';
   console.log(ooo);
   console.log(ob1); //发现了嘛 ，这两个对象的不同在于，ooo的属性名是字符串，而ob1属性名是一个指针。
  
  //接下来来了！！ js最常用类型Array。
  
   //首先理解Array这个函数的使用，它可以接受参数,number类型或者string类型都可，直接作为数组的项
   console.log(Array(1,2,3));// [1,2,3]
  
   //更常用的，数组字面量表示，和用构造函数创建是一个道理 [] == new Array()
   var colors = [1,2,3];
  
   //接下来讲讲这个Array类型的属性
   //首当其冲的就是这个索引,可读可写，从0开始
   console.log(colors[2]) // 3
   console.log(colors[3]) //超过最大索引就为undefined
  
   //然后是length属性，很神奇，也是可读可写
  
   console.log(colors.length);//3
   console.log(colors.length =4);//把数组的length属性改为4
   console.log(colors);//结果真就变4了，最后一项是一个空位，啥也没有。
   colors[colors.length] = 'end'; //因为.length属性始终指向数组最后一项的后一项，所以可以用这个属性来增长数组。
   console.log(colors);
  
  
   // 接下来看看怎么判断一个对象是否是Array类型的实例
   // 想必已经想到了，判断引用类型的种类用instanceof是最好的；
  
   console.log(colors instanceof Array);// true
  
   //但是之前没有讲到一个问题就是，这个instanceof判断是要根据全局执行环境的，就比如一个项目引入了几个框架，每个框架都有自己的全局执行环境，每个框架的数组实例
   //都来自不同的自己框架的Array类型，这就不好判断instanceof 后面该不该只写Array了。
  
   //但是Array类型的isArray()方法可以不管这些。 注意这个方法不是Array原型对象的方法哦，这是存在于Array函数内的方法。
   console.log(Array);
   console.log(Array.isArray(colors));//true
   console.log(Array.prototype);//可以在这里查看Array的所有原型方法。
   console.log(Object.getPrototypeOf(Array.prototype));//顺便提一句，Array原型的原型是Object类型。
  
  
   //那么Array怎么转换成其他类型诶，可以用object类型的方法，也就是每个对象都自带的方法valueOf或者toString
  
   //先来看看显式转换
   console.log(colors.valueOf()); //返回数组本身
   console.log(colors.toString()); //返回以逗号为分隔符的字符串，类似于Array的原型方法.join(',')
   console.log(colors.join(',') == colors.toString()); //true
  
  //再看看隐式转换(复杂数据类型隐式转换时默认先调用valueOf(),若结果为基本数据类型则停止转换，如不是则再toString())
  
   console.log(colors + 1); // 1,2,3,,end1
   console.log(colors + '123');//1,2,3,,end123
  
  
   // 常用的数据结构方法（Array原型方法）
  
   //1.栈方法pop（后进先出）
  
   //pop方法；移除数组末尾最后一项，返回被移除的项
   console.log(colors.pop()); //end
  
   //2.队列方法shift
   //shift方法:移除数组第一项并返回
   console.log(colors.shift());
  
   //这里还有个unshift方法,用于再数组前端添加项，返回数组长度，和push对应
   console.log(colors.unshift('Start'))//4
   //push方法:逐个添加项到数组末尾，返回添加完后的数组长度
   console.log(colors.push('end of end')); //5
   console.log(colors)
  
  
   //3.重排序方法sort and reverse 返回值都为改变后的数组
  
   var values = [5,4,3,2,1];
   console.log(values.sort());//[1,2,3,4,5]
  
   //sort方法默认升序排列数组项，sort会对每一个数组项使用toString转换成字符串之后再进行比较，即使项本身是个数字，最后比较的也是字符串。
   //所以啊sort默认的这个升序排列是不稳定的，比如下列
  
   var values1 = [5,10,4,3,2,1];
   console.log(values1.sort()); //[1,10,2,3,4,5],因为字符串‘10’是在‘5’前面的，根据编码来的好像
  
   //用sort方法时最好自己写比较函数。这个比较函数有两个参数，若是第一个参数在第二个参数之前则返回负数，反之正数，相等为0；
   //下面就是一个简单的升序函数
  
  
   function compare(v1,v2) {
  if(v1<v2){
      return -1;
  }
  else if(v1>v2){
      return 1;
  }
  else{
      return 0;
  }
   }
  
   console.log(values1.sort(compare));//[1,2,3,4,5,10]
  
  // Array的排序方法中还有一个逆序方法 reverse();
  
   console.log(values1.reverse());//[10,5,4,3,2,1]
  
  
  //4.数组操作方法 concat slice splice
  
   //concat 连接方法，参数为要连接的项
  
   var cc = [1,2,3];
   var bb = cc.concat([4,5,6],7);
   console.log(bb)//[1,2,3,4,5,6,7]
  
  //slice 截取方法，接收一到两个参数，起始位置和结束位置，默认一个参数时为起始位置。截取目标是起始位置项 + 起始位置到结束位置之间的项
  //若是不写结束位置，则会截取到最后。
   // 最后返回的是一个截取的新数组；而且对原数组无影响。
   console.log(bb.slice(0)); //[1,2,3,4,5,6,7]
   console.log(bb);//[1,2,3,4,5,6,7]
   console.log(bb == bb.slice(0));//false
  
   console.log(bb.slice(0,1));//[1]
  
   //splice 改写方法，最强大的数组方法。
   //接收的参数根据实际情况而定。
   //第一个参数是起始位置（下标），第二个参数是删除的项数，后面的参数就是新添加或替换的项数
   //直接作用于原数组，改变原数组并返回删除或者是添加的项的数组
  
   //删除第一项(shift)
   console.log(bb.splice(0,1)); //[1]
   console.log(bb);//[2,3,4,5,6,7]
  //从下标位置1开始插入一项
   console.log(bb.splice(1,0,2.5)); // [2.5]
   console.log(bb);//[2,2.5,3,4,5,6,7]
  //从下标位置1开始替换一项
   console.log(bb.splice(1,1,555)); //[555]
   console.log(bb);//[2,555,3,4,5,6,7]
  
  
  
   // 5.位置方法 indexOf() 和 lastIndexOf();
   // 参数则为要查找的项，第一次找到了则就返回下标，没找到返回-1
   // indexOf()从起始位置开始向后找，lastIndexOf()则是后向前
   console.log(bb.indexOf(555)) // 1
   console.log(bb.lastIndexOf(6)) // 5
  
  //6.迭代方法
  //又称数组遍历方法，十分常用且重要。
  //每个方法接受两个参数，一个是要在每一项上运行的函数，一个是运行该函数的作用域对象
  
  //every:对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true
  //some: 对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true
  
  console.log( bb.every(function (item,index,array) {
           return item > 0;
  }));// true
  
  console.log( bb.some(function (item) {
    return item > 500;
  }));// true
  
  
  
  //filter: 对数组中的每一项运行给定函数，返回该函数会返回的true的项组成的数组
   console.log( bb.filter(function (item) {
       return item > 500;
   })) //[555]
  
  
  //forEach: 对数组中的每一项运行给定函数。这个方法没有返回值。
   bb.forEach(function (item) {
       console.log(item+ ' is forEach')
   })
  
  
  //map: 对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组
  console.log(  bb.map(function (item) {
        return item+1;
  }));//[3,556,4,5,6,7,8]
  
  
   //6. 归并函数 reduce 和 reduceRight（反向reduce）
   // 接收四个参数： 前一个值pre 当前值cur 项的索引index 数组对象array
   // 这个函数返回的任何值都会作为第一个参数（pre）自动传给下一项。第一次迭代发生在数组第二项（cur）上。
  
   var sum = bb.reduce(function (pre,cur,index,array) {
      return pre+cur;
   });// 求和
   console.log(sum)
  
  ```

  

## 2020/10/05

**啊国庆放假啊，没有心思学习啦！！！**

**不行，偷偷摸摸还是要学一点点啦**



**因为之前看完了引用类型的Array和Date两个类型**

**正准备看regexp类型时发现自己对正则表达式一窍不通，就去补了一下正则的知识。现在重新来看看这个regexp类型。**

```javascript

 //  啊，要放国庆啦，最近的学习有点点拉跨了。。。 明明才刚开始不久
 // 今天 准备吧引用类型给看完，还剩下RegExp，Date，Function。

 //先开始Date类型

 // 默认不传入参数时，会返回当前日期和时间。
 var date = new Date();
 // 实际上是通过Date类型的方法Date.now();获得的当前时间的毫秒数。

console.log(date +" "+ Date.now())

 //但是也可以根据特定的日期和时间创建日期对象，则要传入表示该日期的毫秒数。
 //这个毫秒数又怎么获得呢？？
 //Date.parse('')方法可以获得参数中的时间字符串的毫秒数表达。时间字符串格式不固定///QWQ///。但是传入的字符串不符合规定则会返回NaN。

 var time = Date.parse('2080/09/09');
 console.log(time);

 console.log(Date);
 console.log(Date.prototype);   //这就可以知道了，Date.parse()是Date类型的方法，并不来自Date的原型。


 //Date.parse()作为Date类型的内置函数，他会在Date对象创建的时候自动使用。
 //例如下面
 var date1 = new Date('2080/09/09');
 console.log(date1);//可以看到是成功创建了2080/09/09日期时间的对象。


 //虽然已经成功了，但是会不会觉得Date.parse()方法并未太精确。因为它并未指明一天的时秒分。
 // 其实还有一个返回日期毫秒数的方法Date.UTC()
 //这个方法所需的参数则是精确的时间。
 // year, month(0 -- 11), day（1 -- 31）, hour（0 -- 23）, minite, second.
 var time1 = Date.UTC(2020,8,29,18,30,20);//2020年9月29日18点30分20秒
 console.log(time1);

//Date类型由于也是继承了Object类型，所以也有toString()和valueOf().

 console.log(date.toString()); //返回自身（字符串形式）
 console.log(date.valueOf()); //返回毫秒数

 //除了上面两个比较常用的，还有很多Date原型的方法，用于日期格式化。
 console.log(date.toDateString());//和toString比较来看只有年月日，没有具体时间;
 console.log(date.toISOString());//又是另一种时间表示的方式。
 /*                等等等等                  */








 /*-----------------------------------------------------分割线-------------------------------------------------------------*/




 //RegExp类型。 让人不明觉厉的类型！
 // RegExp类型可以说是js中正则表达式的容器。
 // ECMAScript通过RegExp类型来支持正则表达式。

 //先来讲讲 正则表达式字面量 和 RegExp构造函数 两种创建正则表达式的方法的 不同点。

 //1. 正则表达式字面量
 //特点:无论给expression赋值多少次，都共享一个RegExp实例。

 //2. RegExp构造函数
 //特点：创建的每一个RegExp实例都是一个新实例。

 //不过啊，ES5过后规定了使用正则表达式字面量时也会直接调用RegExp构造函数一样。所以接下来的代码本来应该有问题，但现在都是true。

 var re = null,i;

 for (i = 0; i < 10; i++){
  re = /cat/g;
  console.log(re.test('catastrophe'));
 }

 for (i = 0; i < 10; i++){
  re = new RegExp('cat','g');
  console.log(re.test('catastrophe'));
 }



// 现在来讲讲RegExp实例属性有哪些
 //常用的g i l m s

 var pa = /[bc]at/ig;
 console.log(pa.global);  // 布尔值， 表示是否设置了g标志。
 console.log(pa.ignoreCase); // 布尔值， 表示是否设置了i表示
 console.log(pa.lastIndex); // 整数， 表示开始搜索下一个匹配项的字符位置，从0算起。
 console.log(pa.multiline); // 布尔值， 表示是否设置了m标志。
 console.log(pa.source);// 正则表达式的字符串表示。


 //实例方法
 //exec()

console.log(pa.exec('cat,bat,pat')); // 第一次检索从0开始，能检索到cat，然后结束。
console.log(pa.exec('cat,bat,pat')); // 第二次检索从4开始（因为g），能检索到bat，然后结束。
console.log(pa.exec('cat,bat,pat')); // 第三次检索从7开始，没能匹配到，返回null。

// 修饰符没有g的话每次exec的时候它的lastindex都是0哦。也就是每次都从开头开始，不会记忆上次结束的地方。

//然后就是RegExp构造函数属性了。
//其实他们都有自己的短属性名，不过这样更直观。

 console.log(RegExp);
 console.log(RegExp.input); // 最近一次要匹配的字符串。
 console.log(RegExp.lastMatch); // 最后一次的匹配项。
 console.log(RegExp.lastParen); // 最后一次匹配的捕获组。
 console.log(RegExp.leftContext); //input字符串中，lastMatch之前的字符串
 console.log(RegExp.rightContext); //input字符串中，lastMatch之后的字符串






```



## 2020/10/09

**回归正常的学习**

