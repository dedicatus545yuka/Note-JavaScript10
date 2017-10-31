# Note-JavaScript10
DOM, Node对象的一些简单的方法
### DOM  
#### `Node`对象 
##### 遍历节点   
1. 获取父节点  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>遍历节点</title>
</head>
<body>
<ul id="parent">
    <li>苹果</li>
    <li id="huawei">华为</li>
    <li>小米</li>
</ul>
<script>
    // 1. 获取id="huawei"节点的父节点
    var child = document.getElementById('huawei');
    /*
        * parentNode - 获取父节点
          * 父级节点可以是所有节点类型，包括文档节点
        * parentElement - 获取父元素节点
          * 父级必须是元素节点 -> 其实就必须是HTML的标签
          * <html>标签的父元素节点是null - 原因在于 <html> 标签的父节点是文档节点。文档节点并不是一个元素节点，所以 parentElement 返回 null。
     */
    // var parent = child.parentNode;
    var parent = child.parentElement;
    console.log(parent);

    var html = document.documentElement;// <html>
    console.log(html.parentNode);
    console.log(html.parentElement);
</script>
</body>
</html>
```
- parentNode: 表示获取指定节点的父节点。一个元素节点的父节点可能是一个元素(Element )节点，也可能是一个文档(Document )节点。
- parentElement: 表示获取当前节点的父元素节点。如果该元素没有父节点，或者父节点不是一个元素节点，则返回 null。

2. 获取子节点  
- firstChild: 获取指定标签的第一个子节点。
- lastChild: 获取指定标签的最后一个子节点。
- childNodes: 获取指定标签的所有子节点。
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取子节点</title>
</head>
<body>
<ul id="parent">
    <li>苹果</li>
    <li id="huawei">
        华为
        <ul>
            <li>MATE 10</li>
        </ul>
    </li>
    <li>小米</li>
</ul>
<script>
    // 获取id="parent"节点的第一个子节点
    var parent = document.getElementById('parent');
    /*var firstChild = parent.firstChild;
    console.log(firstChild);*/

    // 获取id="parent"节点的所有子节点
    /*var children = parent.childNodes;
    console.log(children.length);// 7
    var arr = [];
    for(var i=0;i<children.length;i++){
        var child = children[i];
        /!*
            如何可以判断当前节点的类型
            * nodeName - 用于获取或设置节点的名称
            * nodeType - 用于获取或设置节点的类型
            * nodeValue - 用于获取或设置节点的值
         *!/
        if (child.nodeType === 1){
            arr.push(child);
        }
    }
    console.log(arr);*/

    var li = document.getElementById('huawei');
    var children = li.childNodes;
    console.log(children);
    for (var i=0;i<children.length;i++){
        var child = children[i];
        if (child.nodeType === 3){
            if(child.nodeValue == '    '){
                console.log('这是空白节点');  //这里并不会成功输出，因为这不单纯是换行和空格的问题
            }
        }
    }

</script>
</body>
</html>
```
![判断节点类型](http://a3.qpic.cn/psb?/V118JuTr0BKcy7/4ZvuFEXukhL63y5oQgTInNfXLR8sGRnUhuyYYSVtnt0!/m/dPIAAAAAAAAA&bo=ngJyAQAAAAADB80!&rf=photolist)  

3. 空白节点   
>在`IE8`版本或之前的版本中运行,不会出现空白节点的问题    

![](http://a3.qpic.cn/psb?/V118JuTr0BKcy7/msyTEg.1hat8rQ800iXyyRgkIQju8uHu8jGO51TmFTo!/m/dPIAAAAAAAAA&bo=GwWAAgAAAAADB74!&rf=photolist)    

- 解决空白节点的问题  
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>解决空白节点</title>
</head>
<body>
<ul class="phone">
    <li class="apple">苹果</li>
    <li>小牧</li>
    <li>华为</li>
</ul>
<script>
    var phone = document.getElementsByClassName('phone');//类数组问题
//    console.log(phone[0].firstChild);// #text
//      遍历所有的子节点
//     var children = phone[0].childNodes;
//     console.log(children.length); // 7
//     for(i=0;i<children.length;i++){
//         console.log(children[i]);
//     }
    function FirstChild(parentnode){ // 通过创建一个方法来解决问题
        var child = parentnode.firstChild;
        if(child.nodeType === 3){
            child = child.nextSibling;
        }
        return child;
    }
    var firstchild = FirstChild(phone[0]);// 调用函数
    console.log(firstchild); //  <li class="apple">苹果</li>
</script>
</body>
</html>
```
> 在解决这种浏览器兼容问题的时候可以通过引入外部方案来解决 - MyTool

4. 获取兄弟节点   
- 通过 HTML 页面的指定标签查找兄弟节点，我们可以通过如下属性实现:
    - previousSibling: 获取指定节点的前一个兄弟节点。
    - nextSibling: 获取指定节点的后一个兄弟节点。
##### 插入节点   
- appendChild(): 将一个节点添加到指定父节点的子节点列表末尾。
- insertBefore(): 在当前节点的某个子节点之前再插入一个子节点。
> 除此之外我们还可以将一个已存在 HTML 页面的标签插入到指定标签中，这是原来的标签相当于被挪走了       
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插入节点</title>
</head>
<body>
<ul id="parent">
    <li>苹果</li>
    <li id="mi">小米</li>
    <li id="xigua">西瓜</li>
</ul>
<script>
    // 向<ul>标签添加子节点 <li>香蕉</li>
    /*// 1. 获取目标节点
    var ul = document.getElementById('parent');
    // 2. 创建新的子节点
    var newLi = document.createElement('li');
    var textNode = document.createTextNode('香蕉');
    newLi.appendChild(textNode);
    // 3. 将新的子节点添加到<ul>标签中
    ul.appendChild(newLi);*/

    // parent.appendChild(newChild)方法 -> 将新节点添加到父节点的所有子节点列表的最后面

    // 向id="mi"节点的前面添加一个新的兄弟节点
    /*var ul = document.getElementById('parent');
    var mi = document.getElementById('mi');

    var newLi = document.createElement('li');
    var textNode = document.createTextNode('香蕉');
    newLi.appendChild(textNode);

    ul.insertBefore(newLi,mi);*/

    // parent.insertBefore(newChild, currentChild)方法 -> 将新节点添加指定节点的前面(前一个兄弟节点)

    // 插入/添加节点 -> 没有insertAfter()方法

    // 将新节点添加到指定节点的后面(后一个兄弟节点)
    function insertAfter(parentNode, currentChild){
        /*
            * 参数
              * 作为所有子节点的共同的父级节点
              * 目标节点
         */
        var next = currentChild.nextSibling;
        // 解决空白节点问题
        if (next.nodeType === 3){
            next = next.nextSibling;
        }
        // 判断下一个兄弟节点是否存在
        if (next !== null){
            parentNode.insertBefore(newChild,next);
        } else {
            parentNode.appendChild(newChild);
        }
    }

    var xigua = document.getElementById('xigua');

    console.log(xigua.nextSibling.nextSibling);

</script>
</body>
</html>
```
![伪insertAfter()方法](http://a4.qpic.cn/psb?/V118JuTr0BKcy7/0eprc.XjKfTnz7Pqgh5cGguwyXU2NGMF4LabexlMPbc!/m/dPMAAAAAAAAA&bo=GwWAAgAAAAADB74!&rf=photolist)   

##### 删除节点  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>删除节点</title>
</head>
<body>
<ul id="parent">
    <li>苹果</li>
    <li id="child">小米</li>
    <li>锤子</li>
</ul>
<script>
    var parent = document.getElementById('parent');
    var child = document.getElementById('child');

    parent.removeChild(child);

</script>
</body>
</html>
```
##### 替换节点  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>替换节点</title>
</head>
<body>
<ul id="phone">
    <li>苹果</li>
    <li id="mi">小米</li>
    <li>锤子</li>
</ul>
<ul id="game">
    <li>魔兽</li>
    <li id="wzry">王者荣耀</li>
    <li>绝地求生</li>
</ul>
<script>
    // 新创建的<li>标签
    var newLi = document.createElement('li');
    var text = document.createTextNode('大米');
    newLi.appendChild(text);
    // 从页面中获取到的<li>
    var wz = document.getElementById('wzry');

    var phone = document.getElementById('phone');
    var mi = document.getElementById('mi');

    phone.replaceChild(wz,mi);

</script>
</body>
</html>
```
##### 复制节点   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>复制节点</title>
</head>
<body>
<button id="btn">按钮</button>
<br>
<p id="p1" style="color: lightcoral;">这是一个段落.</p>
<script>
    var btn = document.getElementById('btn');
    /*
        将button按钮进行复制
        * 默认不传递任何参数时 - 没有包含文本内容
        cloneNode(deep)方法
        * deep - 是否复制当前节点的后代节点
          * true - 复制
          * false - 默认值，表示不复制
        * 注意 - 如果被复制的节点具有id属性时 - 复制完成之后，修改id属性的值
      */
    var copy = btn.cloneNode(true);

    var p1 = document.getElementById('p1');
    p1.appendChild(copy);

</script>
</body>
</html>
```
>cloneNode() 方法的参数表示是否采用深度克隆。如果为true，则该节点的所有后代节点也都会被克隆；如果为false，则只克隆该节点本身





