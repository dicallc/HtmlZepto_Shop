###纯h5+css3+zepto移动电商首页


#####UI：

![](https://raw.githubusercontent.com/dicallc/HtmlZepto_Shop/master/images/TIM%E6%88%AA%E5%9B%BE20170817110245.png)

zepto:

> 专门针对移动端浏览器的；因为它的最初目标在移动端提供一个精简的类似jquery的js库。

#### 缺点： ####
1. PC上不容置疑的使用jq，但是移动端又要使用zepto就会产生两份js插件库，大大增加了维护成本

1. 虽然zepto比jQuery小，但其实文件的大小只在第一次打开该网站时有影响，后面都是使用304本地缓存无需重新下载，文件大小的区别已经没影响了。就算是第一次下载，移动端不需要IE可以使用jQuery2以上版本，况且现在网站都用GZip压缩，30K左右的大小也能接受吧。

1. 有一个网站对这几个库进行性能测试，结果是jQuery比zepto的执行效率要高

### API ##

----------

##### 1.获取元素 ###

    $('div')  //=> 所有页面中得div元素
    $('#foo') //=> ID 为 "foo" 的元素

##### 2.创建元素 ###

    $("<p>Hello</p>") //=> 新的p元素
    // 创建带有属性的元素:
    $("<p />", { text:"Hello", id:"greeting", css:{color:'darkblue'} })

##### 3. 回调####
    // 当页面ready的时候，执行回调:
    Zepto(function($){
      alert('Ready to Zepto!')
    })

#####4.animate

    animate(properties, [duration, [easing, [function(){ ... }]]])   ⇒ self
      animate(properties, { duration: msec, easing: type, complete: fn })   ⇒ self
      animate(animationName, { ... })   ⇒ self


- properties: 一个对象，该对象包含了css动画的值，或者css帧动画的名称。

- duration (默认 400)：以毫秒为单位的时间，或者一个字符串。
	- fast (200 ms)
	- slow (600 ms) 
	- 任何$.fx.speeds自定义属性
- easing (默认 linear)：指定动画的缓动类型，使用以下一个：
	- ease
	- linear
	- ease-in / ease-out
	- ease-in-out
	- cubic-bezier(...)
- complete：动画完成时的回调函数

轮播图定时滑动例子:
$(function() {

    // 获取 轮播图的宽度
    var bannerWidth = $('.jd_banner').width();

    // 定时器 自动轮播
    var timeid = setInterval(function() {
        index++;

        // 动画的方式 让 ul 进行移动
        $('.jd_banner ul:first-child').animate({
            transform: 'translateX(' + index * bannerWidth * -1 + 'px)'
        }, 300,'linear' ,function() {
            // console.log('滚动完毕');
            // 判断index 是否越界
            if (index > 8) {
                index = 1;
                $('.jd_banner ul:first-child').css('transform', 'translateX(' + index * bannerWidth * -1 + 'px)');
            }

            // 修改 索引
            $('.jd_banner ul:last-child li').removeClass('now').eq(index - 1).addClass('now');
        });
    }, 2000);



#####Touch

“touch”模块添加以下事件，可以使用 on 和 off。

- tap —元素tap的时候触发。
- singleTap and doubleTap — 这一对事件可以用来检测元素上的单击和双击。(如果你不需要检测单击、双击，使用 tap 代替)。
- longTap — 当一个元素被按住超过750ms触发。
- swipe, swipeLeft, swipeRight, swipeUp, swipeDown — 当元素被划过时触发。(可选择给定的方向)
- 这些事件也是所有Zepto对象集合上的快捷方法。

######轮播图滑动例子:

    $('.jd_banner').swipeLeft(function() {

        // 关闭定时器
        clearInterval(timeid);

        index++;

        // 动画的方式 让 ul 进行移动
        $('.jd_banner ul:first-child').animate({
            transform: 'translateX(' + index * bannerWidth * -1 + 'px)'
        }, 300, function() {
            // console.log('滚动完毕');
            // 判断index 是否越界
            if (index > 8) {
                index = 1;
                $(this).css('transform', 'translateX(' + index * bannerWidth * -1 + 'px)');
            }

            // 修改 索引
            $('.jd_banner ul:last-child li').removeClass('now').eq(index - 1).addClass('now');

            // 过渡结束以后 开启定时器
           timeid = setInterval(function() {
                index++;

                // 动画的方式 让 ul 进行移动
                $('.jd_banner ul:first-child').animate({
                    transform: 'translateX(' + index * bannerWidth * -1 + 'px)'
                }, 300, function() {
                    // console.log('滚动完毕');
                    // 判断index 是否越界
                    if (index > 8) {
                        index = 1;
                        $('.jd_banner ul:first-child').css('transform', 'translateX(' + index * bannerWidth * -1 + 'px)');
                    }

                    // 修改 索引
                    $('.jd_banner ul:last-child li').removeClass('now').eq(index - 1).addClass('now');
                });
            },2000)
        });

    })