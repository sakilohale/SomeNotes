#  Vue学习日志

##  指令中的动态参数

![image-20200911105320743](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911105320743.png)

这个方括号 [] 中间的内容存在于 vue实例中 data property attributeName，是一个动态参数。

这个功能可以自定义填入所需的html的属性，但是【】语法约束比{}多一些。例如里面如果是表达式则不能包含空格。因为浏览器会强制所有html的attribute命名为小写，所以命名时注意不要有大写字母。



##  计算属性Computed

###  因为我计算属性用的少所以写写巩固一下。

####  设计初衷： 使模版内的表达式更加简洁便利，对于任何复杂逻辑，都应当使用计算属性解决

####  基础写法

![image-20200911111002789](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911111002789.png)

因为常常用不到setter，所以有了一下的常用写法

![image-20200911111058415](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911111058415.png)



####  计算属性缓存vs方法

因为不需要计算属性，再methods中创建一个实现复杂逻辑的方法也可以办到同样的事情。例如![image-20200911111527946](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911111527946.png)

但是但是，实际实现时还是选择计算属性！ 因为计算属性的响应式缓存机制，即基于的响应式依赖不变，则每次需要用时就直接从缓存拿而非再次求值。用函数方法的话就会每次都执行一次复杂逻辑，非常吃性能。



## 侦听器

###  Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。有时也可以用来满足一些计算属性的需求。

![image-20200911112319684](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911112319684.png)

除了 `watch` 选项之外，您还可以使用命令式的 [vm.$watch API](https://cn.vuejs.org/v2/api/#vm-watch)。





## styleClass绑定

### vue有很多种绑定class的方法，一下列举一下



#### 对象语法

![image-20200911112903422](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911112903422.png)

这是传入一个对象，然后用了一个判断（注意text-danger用了''是因为-的存在）

结果为

![image-20200911112935644](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911112935644.png)





接下来 升级版

直接传一个data property 的一个对象

![image-20200911113134680](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911113134680.png)





在接下来 升升级版，即为计算属性版

![image-20200911113225118](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911113225118.png)

之前也讲过了计算属性的好处，这里使用计算属性的话就可以避免模板语法复杂化，且有着响应式缓存的好性能。



#### 数组语法

![image-20200911113609541](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911113609541.png)

这是直接渲染，如果想要条件渲染的话可以使用三元表达式。

![image-20200911113724516](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911113724516.png)



有点复杂且鸡肋，所以再数组语法中用对象语法也是可以的呀

![image-20200911113755055](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911113755055.png)

这就是终极形态。

想了想，数组语法好像没有啥优点，所以建议使用对象语法的各类形态。

### 绑定内联样式

![image-20200911120812007](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911120812007.png)

大同小异



## 事件修饰符

![image-20200911195703144](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911195703144.png)

![image-20200911195710633](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200911195710633.png)

https://cn.vuejs.org/v2/guide/events.html

太多了不想复制粘贴了，用的时候自取就行





## 表单绑定

### v-model是用来表单数据绑定的，实际上也是一个语法糖，这里写写它的构造。

```javascript
<input v-model='inputvalue'></input>

//等价于

<input v-bind:value='inputvalue' v-on:input='inputvalue = $event.target.value'></input>

//v-bind 是让value等于inputvalue，v-on 是让inputvalue等于value。
```

```javascript
// 若是再组件上使用略有不同

<blog-post v-model='inputvalue' ></blog-post>

//等价于

<blog-post v-bind:value='inputvalue' v-on:bloginput='$event'></blog-post>

//这里之所以把$event.target.value变成了$event，是因为这个event是在组件内部的表单元素上的，组件是一个大容器，不能引用。这个value其实也是组建的props而非input的value
//所以要做的就是赋值


Vue.component('blog-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('bloginput', $event.target.value)"
    >
  `
})

//v-on 那句意思是，当input事件触发时，触发另一个input事件，并传入参数，值为$event.target.value，可以利用$event回调得到。
```

