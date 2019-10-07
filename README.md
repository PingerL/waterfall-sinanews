
## 瀑布流新闻网站
[预览地址](https://pingerl.github.io/waterfall-sinanews/)
## 懒加载原理
- 懒加载概念：对于页面有很多静态资源的情况下（比如网商购物页面），为了节省用户流量和提高页面性能，可以在用户浏览到当前资源的时候，再对资源进行请求和加载。
- 实现原理：监听onscroll事件判断资源位置：首先为所有懒加载的静态资源添加定义属性字段，比如如果是图片，可以指定data-src为真实的图片地址，src指向loading的图片。然后当资源进入视口的时候，将src属性值替换成data-src的值
## 瀑布流原理
- 瀑布流概念：瀑布流，又称瀑布流式布局。是比较流行的一种网站页面布局，视觉表现为参差不齐的多栏布局，随着页面滚动条向下滚动，这种布局还会不断加载数据块并附加至当前尾部
- 实现原理：JS中，先计算出瀑布流图中元素应当出现的位置，然后通过绝对定位来将元素放置到相应的位置去
        1. 先计算每一行能容纳的列数
        2. 声明一个数组，用来存放每一列的高度之和
        3. 当新加入一个图片元素节点时，遍历比较数组的大小，得到最小的数值minHeight及其下标index，并将新增加的图片元素的高度与原数值相加，得到新的高度数组，下次添加新元素继续当前比较。
        4. 创建并添加新的图片元素节点，通过绝对定位，top值为：minHeight , left值为：nodeImg.width * index
- 小结：先通过计算出一排能够容纳几列元素，然后寻找各列之中所有元素高度之和的最小者，并将新的元素添加到该列上，然后继续寻找所有列的各元素之和的最小者，继续添加至该列上，如此循环下去，直至所有元素均能够按要求排列为止
## 如何判断元素出现在用户视野
1. 获取窗口的高度`winH= $(window).height()`
2. 获取窗口滚动的高度`scrollH=$(window).scrollTop()`
3. 获取元素距离窗口顶部偏移高度`eleH=$el.offset().top`
4. 判断`eleH与winH + scrollH`的大小，若eleH值小于winH + scrollH，则在可视范围内，反之不在可视范围内
## 如何判断浏览器滚动到最底部
- clientHeight：这个元素的高度，占用整个空间的高度，所以，如果一个div有滚动条，那个这个高度则是不包括滚动条没显示出来的下面部分的内容。而只是单纯的DIV的高度。
- offsetHeight：是指元素内容的高度。依照上面的，那这个高度呢就是DIV内部的高度，包括可见部分及以滚动条下面的不可见部分。
- scrollTop：可以理解为滚动条可以滚动的长度。
- 判断滚动条滚动到最底端： offsetHeight == (scrollTop + clientHeight)，如果相等，则浏览器滚动到了底部
```
        function isToBottom(){
            $('.container').on('scroll',function(){
                var $this = $(this),
                viewHeight = $this.height(),
                contentHeight = $this.get(0).scrollHeight,
                scrollTop = $this.scrollTop()
            if(contentHeight == viewHeight + scrollTop){
                cosole.log('浏览器已经滚动到最底部')
            }
            })
        }
```
## 实现原理
1. 获取数据
2. 创建节点，通过瀑布流方式放在页面上
3. 判断是否滚动到底部
## 后期可以改进的地方
1. `scroll`使用函数节流
2. 添加`resize`事件，重新布局
