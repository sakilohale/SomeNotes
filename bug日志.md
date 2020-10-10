# bug日志

# HTMLCollection 累数组对象

**1. 一般来说返回这种对象的方法有**

- **Ele.getElementsByTagName()等**
- **document.scripts //返回所有script标签集合**
- **document.links //返回所有a标签集合**
- **document.images //返回所有img标签集合**
- **document.forms** 
- **tr.cells //返回所有td标签集合**
- **select.options //返回所以select全部选项**

  **但也有特例，比如在mounted之前（虚拟dom的时候）调用document，例如document.getElementsByClassName('')就会返回一个HTMLCollection对象，因为这时候还不能操作dom，所以没有返回一个实际可操作的数组（也有可能啥也返回不了，因浏览器而异）**

**2. 同样拥有length，item(),[].等属性方法。**



