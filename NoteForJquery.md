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

