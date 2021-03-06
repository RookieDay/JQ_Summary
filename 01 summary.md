// jQuery的入口函数
// 作用跟 window.onload 是一样的
// jQuery的入口函数 第一种形式
$(document).ready(function () {});
// 第二种形式, 这是上面这种形式的简写，作用完全一样
$(function() {});


jQuery入口函数与js入口函数的区别（理解）
    js入口函数指的是：window.onload = function() {};
    区别一：书写个数不同
        Js入口函数只能出现一次，出现多次会存在事件覆盖的问题。
        jQuery的入口函数，可以出现任意多次，并不会存在事件覆盖问题。
    区别二：执行时机不同
        Js入口函数是在所有的文件资源加载完成后，才执行。这些文件资源包括：页面文档、外部的js文件、外部的css文件、图片等。
        jQuery的入口函数，是在文档加载完成后，就执行。文档加载完成指的是：DOM树加载完成后，就可以操作DOM了，不用等到所有的外部资源都加载完成。
        文档加载的顺序：从上往下，边解析边执行。

jQuery对象和DOM对象的相互转换:
    DOM对象此处指的是：使用js操作DOM返回的结果。
        var btn = document.getElementById(“btnShow”); // btn就是一个DOM对象   
    jQuery对象此处指的是：使用jQuery提供的操作DOM的方法返回的结果。
        jQuery拿到DOM对象后又对其做了封装，让其具有了jQuery方法的jQuery对象，说白了，就是把DOM对象重新包装了一下。
        （联想：手机和有手机壳的手机，手机就好比是DOM对象，有手机壳的手机就好比是jQuery对象）
        var $btn = $(“#btnShow”); // $btn就是一个jQuery对象
    DOM对象转换成jQuery对象：
        var $btn1 = $(btn); // 此时就把DOM对象btn转换成了jQuery对象$btn1
        // $(document）.ready(function(){}); // 调用入口函数
        // 此处是将document这个js的DOM对象，转换成了jQuery对象，然后才能调用jQuery提供的方法：ready
    jQuery对象转换成DOM对象：
        // 第一种方式
        var btn1 = $btn[0]; // 此时就把jQuery对象$btn转换成了DOM对象btn1 （推荐使用此方式）
        // 第二种方式
        var btn2 = $btn.get(0);// 此时就把jQuery对象$btn转换成了DOM对象btn2
以下模拟Jq对象的内部处理：
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <script>
        window.onload = function () {
            var divs = document.getElementsByTagName("div");
            var obj = {
                0: divs[0],
                1: divs[1],
                2: divs[2],
                length: divs.length
            };
            for(var i = 0; i < obj.length; i++) {
                obj[i].innerHTML = "我是内容";
            }
        }

    </script>
</head>
<body>
<div></div>
<div></div>
<div></div>
</body>
</html>

几个自己经常分不清的选择器：
    空格 后代选择器  $(“#j_wrap li”).css(“color”, “red”);选择id为j_wrap的元素的所有后代元素li
    >    子代选择器  $(“#j_wrap > ul > li”).css(“color”, “red”);选择id为j_wrap的元素的所有子元素ul的所有子元素li
    举例   
        $(document).ready(function(){
            // 子代选择器
            // 符号：>
            // 作用：选择指定元素下面的直接子元素（亲儿子元素）
            //$("#ulItem > li").css("background", "pink");
            //$("#ulItem > li").css("height", "100px");

            // 后代选择器
            // 符号：空格
            // 作用：选择指定元素下面的所有 指定的元素（后代）
            $("#ulItem li").css("background", "pink");
        });

find(selector)	查找指定元素的所有后代元素（子子孙孙）	$(“#j_wrap”).find(“li”).css(“color”, “red”);选择id为j_wrap的所有后代元素li
children()	查找指定元素的直接子元素（亲儿子元素）	$(“#j_wrap”).children(“ul”).css(“color”, “red”);选择id为j_wrap的所有子代元素ul
siblings()	查找所有兄弟元素（不包括自己）	$(“#j_liItem”).siblings().css(“color”, “red”);选择id为j_liItem的所有兄弟元素
parent()	查找父元素（亲的）	$(“#j_liItem”).parent(“ul”).css(“color”, “red”);选择id为j_liItem的父元素
eq(index)	查找指定元素的第index个元素，index是索引号，从0开始	$(“li”).eq(2).css(“color”, “red”);选择所有li元素中的第二个

JQ02:
toggleClass(className) 为指定元素切换类 className，该元素有类则移除，没有指定类则添加。$(selector).toggleClass(“liItem”);
显示和隐藏
    1 show方法
    作用：让匹配的元素展示出来
    // 用法一：
    // 不带参数，作用等同于 css(“display”, ”block”)
    /* 注意：此时没有动画效果 */
    $(selector).show();

    // 用法二：
    // 参数为数值类型，表示：执行动画时长
    /* 单位为：毫秒（ms），参数2000表示动画执行时长为2000毫秒，即2秒钟 */
    $(selector).show(2000);
    
    // 参数为字符串类型，是jQuery预设的值，共有三个，分别是：slow、normal、fast
    /* 跟用法一的对应关系为： */
    /* slow：600ms、normal：400ms、fast：200ms */
    $(selector).show(“slow”);

    // 用法三：
    // 参数一可以是数值类型或者字符串类型
    // 参数二表示：动画执行完后立即执行的回调函数 切记这种设计思想  在执行完动画后的回调函数
    $(selector).show(2000, function() {});

    注意：
    jQuery预设的三组动画效果的语法几乎一致：参数可以有两个，第一个是动画的执行时长，第二个是动画执行完成后的回调函数。
    第一个参数是：可以是指定字符或者毫秒数

    2 hide方法
    作用：让匹配元素隐藏掉
    用法参考show方法
    $(selector).hide(1000); // 1000表示什么？
    $(selector).hide(“slow”);
    $(selector).hide(1000, function(){});
    $(selector).hide();

一、滑入滑出动画
    1滑入动画效果（联想：生活中的卷帘门）
    作用：让元素以下拉动画效果展示出来
    $(selector).slideDown(speed,callback);

    注意：省略参数或者传入不合法的字符串，那么则使用默认值：400毫秒（同样适用于fadeIn/slideDown/slideUp）
    $(selector).slideDown();

    2 滑出动画效果
    作用：让元素以上拉动画效果隐藏起来
    $(selector).slideUp(speed,callback);

    3滑入滑出切换动画效果
    $(selector).slideToggle(speed,callback);
    参数等同与隐藏和显示

二、淡入淡出动画
    1 淡入动画效果
    作用：让元素以淡淡的进入视线的方式展示出来
    $(selector).fadeIn(speed, callback);
    2 淡出动画效果
    作用：让元素以渐渐消失的方式隐藏起来
    $(selector).fadeOut(1000);
    3淡入淡出切换动画效果
    作用：通过改变不透明度，切换匹配元素的显示或隐藏状态
    $(selector).fadeToggle('fast',function(){});
    参数等同与隐藏和显示

    4改变不透明度到某个值
    与淡入淡出的区别：淡入淡出只能控制元素的不透明度从 完全不透明 到完全透明；而fadeTo可以指定元素不透明度的具体值。并且时间参数是必需的！
    作用：调节匹配元素的不透明度 
    // 用法有别于其他动画效果
    // 第一个参数表示：时长
    // 第二个参数表示：不透明度值，取值范围：0-1
    $(selector).fadeTo(1000, .5); //  0全透，1全不透
    // 第一个参数为0，此时作用相当于：.css(“opacity”, .5);
    $(selector).fadeTo(0, .5);


停止动画
    //作用：停止当前正在执行的动画
    $(selector).stop();
    为什么要停止动画？
    如果一个以上的动画方法在同一个元素上调用，那么对这个元素来说，后面的动画将被放到效果队列中。这样就形成了动画队列。（联想：排队进站）
    动画队列里面的后续动画不会执行，直到第一个动画执行完成。
    注意：如果一个元素的动画还没有执行完，此时调用sotp()方法，那么动画将会停止，回调函数也不会被执行。
    有规律的体现：
    jQuery提供的这几个动画效果控制的CSS属性包括：高度、宽度、不透明度。{height:400px; width:300px; opacity:0.4;}
    这三个CSS属性的共性是：属性值只有一个，并且这个值是数值（去掉单位后）。

自定义动画
    注意：所有能够执行动画的属性必须只有一个数字类型的值。
    比如：要改变字体大小，要使用：fontSize（font-size），不要使用：font

    动画支持的属性：
        http://www.w3school.com.cn/jquery/effect_animate.asp

    作用：执行一组CSS属性的自定义动画
    // 第一个参数表示：要执行动画的CSS属性（必选）
    // 第二个参数表示：执行动画时长（可选）
    // 第三个参数表示：动画执行完后立即执行的回调函数（可选）
    $(selector).animate({params},speed,callback);


jQuery节点操作
动态创建元素
    // $()函数的另外一个作用：动态创建元素
    var $spanNode = $(“<span>我是一个span元素</span>”);
	
添加元素
在元素的最后一个子元素后面追加元素：append()（重点）
作用：在被选元素内部的最后一个子元素（或内容）后面插入内容（存在或者创建出来的元素都可以）
如果是页面中存在的元素，那么调用append()后，会把这个元素从原先的位置移除，然后再插入到新的位置。
	 如果是给多个目标追加一个元素，那么append()方法的内部会复制多份这个元素，然后追加到多个目标里面去。
常用参数：htmlString 或者 jQuery对象

// 在$(selector)中追加$node
$(selector).append($node);
// 在$(selector)中追加div元素，参数为htmlString
$(selector).append('<div></div>');


不常用操作：（用法跟append()方法基本一致）
    1 appendTo()
    作用：同append()，区别是：把$(selector)追加到node中去
    $(selector).appendTo(node);

    2 prepend()
    作用：在元素的第一个子元素前面追加内容或节点
    $(selector).prepend(node);

    3 after()
    作用：在被选元素之后，作为兄弟元素插入内容或节点
    $(selector).after(node);

    4 before()
    作用：在被选元素之前，作为兄弟元素插入内容或节点
    $(selector).before(node);

html创建元素（推荐使用，重点）
    作用：设置或返回所选元素的html内容（包括 HTML 标记）
    设置内容的时候，如果是html标记，会动态创建元素，此时作用跟js里面的 innerHTML属性相同
    // 动态创建元素
    $(selector).html(‘<span>传智播客</span>’);
    // 获取html内容
    $(selector).html();
    // 传入一个空字符串，表示清空内容
    $(selector).html(“”);

清空元素
    // 清空指定元素的所有子元素（光杆司令）
    // 没有参数
    $(selector).empty();
    // “自杀” 把自己（包括所有内部元素）从文档中删除掉
    $(selector).remove();

复制元素
    作用：复制匹配的元素
    // 复制$(selector)所匹配到的元素
    // 返回值为复制的新元素
    $(selector).clone();

总结：
	推荐使用.html(“<span></span>”)方法来创建元素或者html("")清空元素

操作属性、值和内容
属性操作
    设置属性：
    // 第一个参数表示：要设置的属性名称
    // 第二个参数表示：改属性名称对应的值
    $(selector).attr(“title”, “传智播客”);
获取属性：
    // 参数为：要获取的属性的名称，改操作会返回指定属性对应的值
    $(selector).attr(“title”);

移除属性：
    // 参数为：要移除的属性的名称
    $(selector).removeAttr(“title”); 
注意：checked、selected、disabled要使用.prop()方法。
prop方法通常用来控制DOM元素的动态状态，而不是改变的HTML属性。例如：input和button的disabled特性，以及checkbox的checked特性。
细节参考：http://api.jquery.com/prop/

值和内容
val()方法：
    作用：设置或返回表单元素的值，例如：input,select,textarea的值
    // 获取匹配元素的值，只匹配第一个元素
    $(selector).val();
    // 设置所有匹配到的元素的值
    $(selector).val(“具体值”);

text() 方法:
    作用：设置或获取匹配元素的文本内容
    //获取操作不带参数（注意：这时候会把所有匹配到的元素内容拼接为一个字符串，不同于其他获取操作！）
    $(selector).text();
    //设置操作带参数，参数表示要设置的文本内容
    $(selector).text(“我是内容”);
    
sublime 设置打开浏览器：
选择preferences --- key binding --user 复制下面内容到json数组里面
    //ie
     { 
     	"keys": ["f12"], 
     	"command": "side_bar_files_open_with", 
     	"args": 
     	{ 
     		"paths": [], 
     		"application": "C:/Program Files/Internet Explorer/iexplore.exe",
     		 "extensions": ".*" 
     	} 
     }, 
    //chorme 
    { 
    	"keys": ["alt+f12"], 
    	"command": "side_bar_files_open_with", 
    	"args": 
    	{ 
    		"paths": [], 
    		"application": "C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe", 
    		"extensions": ".*" 
    	} 
    } 


JQ 事件绑定：
    历程：简单事件绑定 >> bind事件绑定 >> delegate事件绑定 >> on
   
    	简单事件绑定：
    click(handler) 				单击事件
    blur(handler) 				失去焦点事件
    mouseenter(handler) 		鼠标进入事件
    mouseleave(handler)			鼠标离开事件
    dbclick(handler) 			双击事件
    change(handler) 改变事件，如：文本框值改变，下来列表值改变等
    focus(handler) 				获得焦点事件
    keydown(handler) 			键盘按下事件
    其他参考：http://www.w3school.com.cn/jquery/jquery_ref_events.asp
   
    	bind方式（不推荐，1.7以后的jQuery版本被on取代）
    作用：给匹配到的元素直接绑定事件
    // 绑定单击事件处理程序
    第一个参数：事件类型
    第二个参数：事件处理程序
    $("p").bind("click mouseenter", function(e){
        //事件响应方法
    });
    比简单事件绑定方式的优势：
        可以同时绑定多个事件，比如：bind(“mouseenter  mouseleave”, function(){})
    缺点：要绑定事件的元素必须存在文档中。

    delegate方式（特点：性能高，支持动态创建的元素）
    作用：给匹配到的元素绑定事件，对支持动态创建的元素有效
    // 第一个参数：selector，要绑定事件的元素
    // 第二个参数：事件类型
    // 第三个参数：事件处理函数
    $(".parentBox").delegate("p", "click", function(){
        //为 .parentBox下面的所有的p标签绑定事件
    });
    与前两种方式最大的优势：减少事件绑定次数提高效率，支持动态创建出来的元素绑定事件！

    on方式（最现代的方式，兼容zepto(移动端类似jQuery的一个库))
    jQuery1.7版本后，jQuery用on统一了所有的事件处理的方法
    作用：给匹配的元素绑定事件，包括了上面所有绑定事件方式的优点
    语法：
    // 第一个参数：events，绑定事件的名称可以是由空格分隔的多个事件（标准事件或者自定义事件）
    // 第二个参数：selector, 执行事件的后代元素
    // 第三个参数：data，传递给处理函数的数据，事件触发的时候通过event.data来使用
    // 第四个参数：handler，事件处理函数
    $(selector).on(events[,selector][,data],handler);

        // 表示给$(selector)绑定事件，当必须是它的内部元素span才能执行这个事件
    $(selector).on( "click","span", function() {});

    // 绑定多个事件
    // 表示给$(selector)匹配的元素绑定单击和鼠标进入事件
    $(selector).on(“click mouseenter”, function(){});

JQ事件解绑
    	unbind() 方式
        作用：解绑 bind方式绑定的事件
        $(selector).unbind(); //解绑所有的事件
        $(selector).unbind(“click”); //解绑指定的事件
    	undelegate() 方式
        作用：解绑delegate方式绑定的事件
        $( selector ).undelegate(); //解绑所有的delegate事件
        $( selector).undelegate( “click” ); //解绑所有的click事件

    off解绑on方式绑定的事件（重点）
        // 解绑匹配元素的所有事件
        $(selector).off();
        // 解绑匹配元素的所有click事件
        $(selector).off(“click”);
        // 解绑所有代理的click事件，元素本身的事件不会被解绑 
        $(selector).off( “click”, "**" ); 

JQ事件触发
    简单事件触发
    $(selector).click(); //触发 click事件
    trigger方法触发事件
    $(selector).trigger(“click”);
    triggerHandler触发 事件处理函数，不触发浏览器行为
    比如:文本框获得焦点的默认行为
    $(selector).triggerHandler(“focus”);

jQuery事件对象
    event.data 					传递给事件处理程序的额外数据
    event.currentTarget 		等同于this，当前DOM对象
    event.pageX 				鼠标相对于文档左部边缘的位置
    event.target 				触发事件源，不一定===this
    event.stopPropagation()；   阻止事件冒泡
    event.preventDefault(); 	阻止默认行为
    event.type 					事件类型：click，dbclick…
    event.which 				鼠标的按键类型：左1 中2 右3
    event.keyCode				键盘按键代码

JQ链式编程： 原理---return this;
    隐式迭代:在方法的内部会为匹配到的所有元素进行循环遍历，执行相应的方法；而不用我们再进行循环，简化我们的操作，方便我们调用。
    如果获取的是多元素的值，大部分情况下返回的是第一个元素的值。$(“li”).css(“font-size”); // 获取的是第一个li的样式

    each方法
    有了隐式迭代，为什么还要使用each函数遍历？
    大部分情况下是不需要使用each方法的，因为jQuery的隐式迭代特性。
    如果要对每个元素做不同的处理，这时候就用到了each方法

    作用：遍历jQuery对象集合，为每个匹配的元素执行一个函数
    // 参数一表示当前元素在所有匹配元素中的索引号
    // 参数二表示当前元素（DOM对象）
    $(selector).each(function(index,element){});


多库共存
    此处多库共存指的是：jQuery占用了$ 和jQuery这两个变量。当在同一个页面中引用了jQuery这个js库，并且引用的其他库（或者其他版本的jQuery库）中也用到了$或者jQuery这两个变量，那么，要保证每个库都能正常使用，这时候就有了多库共存的问题。

    // 模拟另外的库使用了 $ 这个变量
    // 此时，就与jQuery库产生了冲突
    var $ = { name : “itecast” };

    解决方式：
    // 作用：让jQuery释放对$的控制权，让其他库能够使用$符号，达到多库共存的目的。此后，只能使用jQuery来调用jQuery提供的方法
        $.noConflict();
    举例：
        <script>
            /*var $ = 123;

            console.log($);
            // noConflict 释放对$符号的控制权
            $.noConflict();
            jQuery(function () {
                jQuery("li").css("color", "red");
            });*/
            // 如果 $和jQuery这两个都被占用了，那么还可以用 noConflict
            var J = $.noConflict();
            console.log(J);
            var $ = 123;
            var jQuery = 456;
        </script>   

jQuery插件机制
    jQuery这个js库，虽然功能强大，但也不是面面俱到包含所有的功能。
    jQuery是通过插件的方式，来扩展它的功能：
    当你需要某个插件的时候，你可以“安装”到jQuery上面，然后，使用。
    当你不再需要这个插件，那你就可以从jQuery上“卸载”它。
    （联想：手机软件，安装的app就好比插件，用的时候安装上，不用的时候卸载掉）
    

    第三方插件
        jQuery.color.js
        animate()自定义动画：不支持背景的动画效果
        animate动画支持的属性列表
        jQuery.lazyload.js
            使用步骤：
                1.	引入jQuery文件
                2.	引入插件
                3.	使用插件
    制作插件
        如何创建jQuery插件：
        http://learn.jquery.com/plugins/basic-plugin-creation/

        给全局jQuery函数扩展方法
        $.pluginName = function() {};
        // 调用
        $.pluginName();

        给jQuery对象扩展方法
        $.fn.pluginName = function() {};
        $(“div”).pluginName();


jQueryUI
    jQueryUI专指由jQuery官方维护的UI方向的插件。
    官方API：http://api.jqueryui.com/category/all/
    其他教程：jQueryUI教程
    基本使用:
        1.	引入jQueryUI的样式文件
        2.	引入jQuery
        3.	引入jQueryUI的js文件
        4.	使用jQueryUI功能
            
  // 函数声明
  function aa() {}
  // 函数表达式
  var func = function funcName (num1) {
    console.log(funcName);
  };

切记：
window.onload = function(){
    var lis = document.getElementByTagName("li");
    for(var i = 0 ; i < lis.length;i++) {
        lis[i].onclick = (function(){
            return function(){
            alert(num);
            }
        })(i);
    }    
}