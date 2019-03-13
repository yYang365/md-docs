# jQuery代码规范与最佳实践
> [http://www.runoob.com/w3cnote/jquery-style-guide-html.html](http://www.runoob.com/w3cnote/jquery-style-guide-html.html)

分类 编程技术  
以下是书写jQuery代码的基本规范和最佳实践，这些不包括原生JS规范与最佳实践。

![jquery](http://www.runoob.com/wp-content/uploads/2014/05/jquery.jpg)

## 加载jQuery

1、尽量使用CDN加载jQuery。

    <script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
    <script>window.jQuery || document.write('<script src="js/jquery-1.11.0.min.js" type="text/javascript"><\/script>')</script>

百度公共CDN库在国内所有地区速度应该是最快的，js库也很多，推荐大家使用。

    <script src="http://libs.baidu.com/jquery/1.8.3/jquery.min.js"></script>

又拍云JS库加速服务在国内的访问速度还不错，可惜js库比较少。

    <script src="http://upcdn.b0.upaiyun.com/libs/jquery/jquery-1.8.3.min.js"></script>

新浪JS/CSS库又称新浪SAE公共库，在国内的访问速度还不错，js库也很全，推荐大家使用。

    <script src="http://lib.sinaapp.com/js/jquery/1.8.3/jquery.min.js"></script>

## jQuery变量

1. 所有使用或缓存jQuery对象的变量应该以$开头。
2. 始终将jQuery选择器返回的对象缓存到本地变量中以复用。

```
      var $myDiv = $("#myDiv");
      $myDiv.click(function(){....});
```

3. 使用峰驼式命名变量。

## 选择器

1. ID选择器可用时就使用ID选择,它在内部使用document.getElementById()。
2. 当使用类/伪类选择器时，总是给选择器附上元素类型来避免扫描整个DOM树。

```
    // BAD 在整个DOM树中扫描"products"类名
    var $products = $(".products");

    // GOOD 只在DIV元素中扫描"products"类名
    var $products = $("div.products");
```

3. 在ID > 子节点层级选择器中使用find()方法。因为前半部分选择器没使用到Sizzle选择器引擎来查找元素。

```
    // BAD, Sizzle选择器引擎查找层级
    var $productIds = $("#products div.id");

    // GOOD, 只有div.id走Sizzle选择器引擎
    var $productIds = $("#products").find("div.id");
```

4. 选择器后半部分比前半部分明确。

```
    // 未优化
    $("div.data .gonzalez");

    // 优化
    $(".data td.gonzalez");
```

5. 避免冗余选择器。

```
    $(".data table.attendees td.gonzalez");

    // Better: 有必要时要去掉中间不必要的内容
    $(".data td.gonzalez");
```

6. 给选择器添加上下文。

```
    // 要扫描整个DOM树寻找
    $('.class');

    // 只在#class-container里扫描
    $('.class', '#class-container');
```

7. 避免使用通配符选择器。

```
    $('div.container > *'); // BAD
    $('div.container').children(); // BETTER
```

8. 避免使用隐式通配选择器。当省略下面的input时，会隐式的使用通配符选择器。

```
    $('div.someclass :radio'); // BAD
    $('div.someclass input:radio'); // GOOD
```

9. ID选择器使用的是document.getElementById()，ID选择器无需嵌套ID。

```
    $('#outer #inner'); // BAD
    $('div#inner'); // BAD
    $('.outer-container #inner'); // BAD
    $('#inner'); // GOOD
```

## DOM操作

1. 始终先detach现有DOM元素后进行操作，随后将其attach到DOM中。

```
    var $myList = $("#list-container > ul").detach();
    //...针对$myList的许多DOM操作
    $myList.appendTo("#list-container");
```

2. 使用字符串连接或者array.join()而不是.append()方法。

```
    // BAD
    var $myList = $("#list");
    for(var i = 0; i < 10000; i++){
        $myList.append("<li>"+i+"</li>");
    }

    // GOOD
    var $myList = $("#list");
    var list = "";
    for(var i = 0; i < 10000; i++){
        list += "<li>"+i+"</li>";
    }
    $myList.html(list);

    // EVEN FASTER
    var array = []; 
    for(var i = 0; i < 10000; i++){
        array[i] = "<li>"+i+"</li>"; 
    }
    $myList.html(array.join(''));
```

3. 不操作未知元素。

```
    // BAD: 这个函数内部要先执行3个函数，才发现选择器选择到的可能是空内容
    $("#nosuchthing").slideUp();

    // GOOD
    var $mySelection = $("#nosuchthing");
    if ($mySelection.length) {
        $mySelection.slideUp();
    }
```

## 事件

1. 每个页面只使用一个Document Ready函数。利于调试。
2. 不要使用匿名函数绑定事件。匿名函数不利于调试、维护、测试和复用。

```
    $("#myLink").on("click", function(){...}); // BAD

    // GOOD
    function myLinkClickHandler(){...}
    $("#myLink").on("click", myLinkClickHandler);
```

3. Document ready函数不应该是匿名函数。匿名函数不能复用也无法对其测试。

```
    $(function(){ .. }); // BAD: 不容易复用也不利于测试

    // GOOD
    $(initPage); // or $(document).ready(initPage);
    function initPage(){
    // Document ready里可以初始化变量和调用其他初始化函数
    }
```

4. Document ready函数应该放在外部文件里，在其他初始化设置之后，在行内JS里调用这些函数。

```
    <script src="my-document-ready.js"></script>
    <script>
    // 任何其他需要设置的全局变量
      $(document).ready(initPage); // or $(initPage);
    </script>
```

5. 不要在HTML文件里添加行为（行内JS），这是调试的噩梦。始终使用jQuery绑定事件后面会很容易去解绑事件。

```
    <a id="myLink" href="#" onclick="myEventHandler();">my link</a> <!-- BAD -->
    $("#myLink").on("click", myEventHandler); // GOOD
```

6. 如有需要，对事件使用自定义命名空间。这有利于去解绑某DOM元素上特定的事件而不会影响到该DOM元素上的其他事件。

```
    $("#myLink").on("click.mySpecialClick", myEventHandler); // GOOD
    // 后面会很容易的解绑这个特定的点击事件
    $("#myLink").unbind("click.mySpecialClick");
```

## Ajax

1. 避免使用.getJSON()和.get()，只使用$.ajax()，前两者都是在内部使用的后者。
2. 不要在https的网站上使用http请求。侧重无模式的url（在URL上去掉http/https）。
3. 不要把请求参数放在URL里，而是放在data对象里去。

```
    // 不可读
    $.ajax({
        url: "something.php?param1=test1&param2=test2",
        ....
    });

    // 可读
    $.ajax({
        url: "something.php",
        data: { param1: test1, param2: test2 }
    });
```

4. 明确设置数据的类型dataType，这样很容易知道当前正在处理什么样的数据。
5. 对Ajax加载的DOM元素绑定事件时尽量使用事件代理。事件代理的优势是对于Ajax后来添加到DOM的元素也能响应事件。

    $("#parent-container").on("click", "a", delegatedClickHandlerForAjax);

6. 使用Promise

```
    $.ajax({ ... }).then(successHandler, failureHandler);

    // OR
    var jqxhr = $.ajax({ ... });
    jqxhr.done(successHandler);
    jqxhr.fail(failureHandler);
```

7. Ajax模版代码：

```
    var jqxhr = $.ajax({
        url: url,
        type: "GET",      // 默认值GET，可根据需要配置
        cache: true,      // 默认值true, dataType是'script'或'jsonp'时为false，可根据需要配置
        data: {},         // 请求参数对象
        dataType: "json", // 设置数据类型
        jsonp: "callback",// 只在操作JSONP时设置此项
        statusCode: {     // 针对特定错误码的回调处理函数
            404: handler404,
            500: handler500
        }
    });
    jqxhr.done(successHandler);
    jqxhr.fail(failureHandler);
```

## 效果和动画

1. 采用统一的动画实现方式。
2. 不要过度使用动画效果，除非追求用户体验。
    * 尽量使用简单的show/hide/toggle/slideUp/slideDown方法来显示隐藏元素。
    * 尽量使用预定义的动画时间间隔：slow,fast或400

## 插件

1. 始终选择有良好维护、优秀文档、良好测试和社区支持的插件。
2. 细心检查该插件与正在使用的jQuery版本的兼容性。
3. 任何通用的组件都应该抽取成jQuery插件。

## 链式写法

1. 尽量使用链式写法而不是用变量缓存或者多次调用选择器方法。

    $("#myDiv").addClass("error").show();

2. 当链式写法超过三次或者因为事件绑定变得复杂后，使用换行和缩进保持代码可读性。

    $("#myLink")

3. 对于更长的链式调用，可接受用变量缓存一个中间对象。

## 其他原则

1. 参数尽量使用对象字面量。

```
    $myLink.attr("href", "#").attr("title", "my link").attr("rel", "external"); // BAD
    // GOOD
    $myList.attr({
        href: "#",
        title: "my link",
        rel: "external"
    });
```

2. 不要把CSS混进jQuery

```
    $("#mydiv").css({'color':red, 'font-weight':'bold'}); // BAD

    .error { color: red; font-weight: bold; } /* GOOD */
    $("#mydiv").addClass("error"); // GOOD
```

3. 不要使用遭弃用的方法。
4. 需要时可使用原生JS方法。

```
    $("#myId"); // 还是慢了些
    document.getElementById("myId");
```

相关资源
jQuery 教程