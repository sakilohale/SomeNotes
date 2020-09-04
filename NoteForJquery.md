# Jquery 常用的一些api总结

## .addClass()

### 用法：为匹配的元素添加指定的样式

```javascript
$("h1").addClass("box")
```

## .attr()

### 用法：获取匹配的元素集合中的属性的值。然后根据需求设置匹配元素的一个或多个属性。

```javascript
  var title=$("em").attr("title");
  $('div').text(title);
```

## .hasClass()

### 用法：确认任何一个匹配元素是否有被分配给定的样式类

```javascript
$("div").hasClass('box')//返回boolean值
```

## .html()

### 用法：获取集合中第一个匹配元素的HTML内容，设置每一个匹配元素的html内容

```javascript
var html=$('div').html();

$("p").html("<b>Single:</b> " + singleValues + " <b>Multiple:</b> " + multipleValues.join(", ")); }
```


## .prop()

### 用法：获取匹配的元素集中第一个元素的属性值并设置。

```javascript
$("input").prop("disabled", false);
$("input").prop("checked", true);
//切记attribute 和 prop不一样
```

## .val()

### 用法：获取匹配的元素集合中第一个元素的当前值或设置匹配的元素集合中每个元素的值（常用来获取或修改表单元素的内容）

```javascript
    $("input").keyup(function () {
      var value = $(this).val();
      $("p").text(value);
    }).keyup();
//数据绑定jquery版
```

## .toggleClass()

### 用法：添加或删除一个或多个样式类（存在即删除，不存在就即添加）

```javascript
$('p').click(function () {
    $(this).toggleClass('box')
})
```











#  数据处理

##  队列

###  .queue()

![image-20200904191812559](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904191812559.png)

####  常用.queue()来显示在匹配的元素上的已经执行的函数列队（并且可用一些方法类似于.length），.queue（function()）可以在队列中插入自定义函数





###  .dequeue()



![image-20200904192144942](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904192144942.png)

#### 常用于.queue(function(){ /*xxxxxxx*/    ;  $.dequeue(this);  })  从而移除自定义插入的函数并且继续queue队列类函数的发生



### clearQueue()

![image-20200904192502182](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904192502182.png)





```javascript
//一个动画实例
 var div = $("div");

    function runIt() {
        div.show("slow");
        div.animate({left:'+=200'},2000);
        div.slideToggle(1000);
        div.slideToggle("fast");
        div.animate({left:'-=200'},1500);
        //.queue()
        div.queue(function () {
           alert("插入一个函数");
            //移除自身
           $.dequeue(this);
        });
        
        div.hide("slow");
        div.show(1200);
        div.slideUp("normal", runIt);
    }

    function showIt() {
        var n = div.queue("fx");
        $("span").text( n.length );
        setTimeout(showIt, 100);
    }

    runIt();
    showIt();
```







## data

### .data()

![image-20200904214246134](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904214246134.png)

![image-20200904214313336](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904214313336.png)

类似于一下例子

![image-20200904214344276](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904214344276.png)



###  jquery.hasdata()

![image-20200904214936792](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904214936792.png)

.



### jquery.data()

![image-20200904215053924](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904215053924.png)





##  集合函数



###  .each()

![image-20200904215852218](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904215852218.png)



###  .jquery.params()

![image-20200904215935323](C:\Users\19823\AppData\Roaming\Typora\typora-user-images\image-20200904215935323.png)