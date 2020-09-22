#    Vue学习日志

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







## VUE的生命周期*

**文档图示**

![image-20200922124340245](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922124340245.png)

**所有钩子函数有：**

- beforeCreate

- created

- beforeMount

- mounted

- beforeUpdate

- updated

- beforeDestroy

- destroyed

  

### 1. beforeCreate

<img src="C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922130106903.png" alt="image-20200922130106903" style="zoom:200%;" />

**可以看到这个时候正在初始化事件，进行数据的观测（遍历data对象下所有属性并将其转化为getter和setter，也就是为其添加一个被观察者对象【被观察者对象： 观测并连接data内属性和视图的关系】，所以在后来添加新的data的属性时视图不更新，就是因为没有被放到观察者对象中去。解决方法为用$set方法来新增属性）。**



**有点疑惑的是这时候打印出来的data明明是undefined，怎么还能有这些操作。这是因为data是函数，这个时候data没执行所以没有返回数据所以是undefined**

**![image-20200922135944553](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922135944553.png)但是用该方法可以看到data函数是存在的![image-20200922140021235](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922140021235.png)**





### 2. created

![image-20200922130219190](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922130219190.png)

**created状态中，vue实例已经创建完成完毕，数据已经和data属性进行了绑定且可使用。此时dom不存在，无法引用$el。（放入data中的属性当值发生改变的同时，视图层view也会发生变化，不过不是这个阶段）**

**这个生命周期可以进行axios请求，但因为这时页面还未渲染，所以请求过长会导致比较久的白屏，建议加上Loading**





#### 2. created — beforeMount

![image-20200922130511688](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922130511688.png)

**这个时候发生的事情如图所示**

**1.判断vue对象是否有el选项。如果有的话就继续向下编译，没有的话就暂时停止生命周期，直到在该vue实例上调用vm.$mount(el) — 也就是挂载上el，然后继续下一步**

**2.完成上一步后，判断该vue对象中是否有template选项。如果有则如下图![image-20200922130942443](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922130942443.png)**

- **如果vue实例对象中有template参数选项，则将其作为模板编译成render函数**

- **如果没有template选项，则将外部HTML作为模板编译。**

- **所以可知： template模板优先级高于outer HTML**

- **但当vue实例对象又render函数时，该render渲染优先级最高![image-20200922132059698](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922132059698.png)**

- **综上所述： render函数选项 > template选项 > outer HTML**

  



**总而言之这一阶段就是判断+模板编译**

### 3. beforeMount



**此时的状态**![image-20200922131537851](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922131537851.png)

**按常理讲，这里应该还没有$el。这里打印出来的是在created到beforeMount之间时期创建的虚拟DOM，无法被操作和渲染数据**



**关于el![image-20200922131829717](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922131829717.png)**



#### 3. beforeMount—mounted

![image-20200922132814679](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922132814679.png)

**可以看到这时是给vue实例对象添加$el成员，并且替换掉之前掉挂在vue实例对象的虚拟DOM**

**注意以下截图**

![image-20200922133052974](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922133052974.png)

**在mounted之前这个h1中还是通过{{message}}进行占位的，这说明什么，说明此时是虚拟dom，无法进行操作和渲染。而mounted状态{{message}}被替换成了真正的内容，这是完成了真正的挂载。**



### 4. mounted

**这个时候，实例挂载完成完毕，dom渲染完毕。 是最适合进行一些数据请求，数据赋值操作的时期（因为拿到数据后就可以进行下一步渲染）**

**在mounted时期更新被观察者对象内的属性时，会触发beforeUpdate和updated**



**<u>注意，以上四个生命周期阶段，每个组件只会经历一次。</u>**



### 5. beforeUpdate — updated

![image-20200922142805732](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922142805732.png)

****

**beforeUpdate时期会响应被观察者对象内属性的变化并更新其值，但view层并未重新渲染，这时最后一个能够更新属性的机会。**



**Update时期是在view层重新渲染后触发。所以不能再这个时期进行属性的更新，因为更新又触发beforeUpdate，然后又触发Update，最后死循环。**



### 6. beforeDestory — destoryed

![image-20200922143726505](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200922143726505.png)

**beforeDestory在实例销毁前调用，到这一步实例也是可以使用的。**

**destoryed是在实例销毁后调用，这时候vue实例内的所有东西都解绑了。**

