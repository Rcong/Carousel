# 3D轮播Demo

一个3D轮播demo,[戳这里](http://book.jirengu.com/Rcong/my-practical-code/vue-carousel/carousel.html).
![截图](http://oa7h77y2u.bkt.clouddn.com/3d_carousel.gif)

# 如何使用

该轮播```jQuery```，除此之外只涉及到一个```js```文件和```css```文件，用普通方式引入即可。

```javascript

<link rel="stylesheet" href="css/carousel.css">

...

<script src="js/libs/jquery.js"></script>

<script src="js/carousel.js"></script>

```

然后，```new```一个```VCarousel```对象，传入相应参数。

```javascript

<body>

    <div class="wrap">

        <div id="app"></div>

    </div>

</body>

...

var vcarousel = new VCarousel({

    wrap: $('#app'),

    data:[{

                url: 'https://github.com/Rcong',

                img: 'http://7xrunf.com1.z0.glb.clouddn.com/rcong-img0.jpg'

            }, {

                url: 'https://github.com/Rcong',

                img: 'http://7xrunf.com1.z0.glb.clouddn.com/rcong-img1.jpg'

            }, {

                url: 'https://github.com/Rcong',

                img: 'http://7xrunf.com1.z0.glb.clouddn.com/rcong-img2.jpg'

            }, {

                url: 'https://github.com/Rcong',

                img: 'http://7xrunf.com1.z0.glb.clouddn.com/rcong-img3.jpg'

            }, {

                url: 'https://github.com/Rcong',

                img: 'http://7xrunf.com1.z0.glb.clouddn.com/rcong-img4.jpg'

            }]

});

```



# 实现思路

![carousel结构图](http://7xrunf.com1.z0.glb.clouddn.com/carousel%E7%BB%93%E6%9E%84%E5%9B%BE.png)

上图就是整个demo的结构图，整体有一个```class```为```carousel```的容器用来包裹```class```为```img-list```轮播图容器和```class```为```switch-list```的切换按钮容器。



设定```carousel```的宽度和高度，在这个demo中为高为194px，宽为765px，然后设置```overflow: hidden```隐藏内部超出部分。内部的```img-list```宽度设置和父容器一样长也为765px，高度设置为168px，并且设置```position: relative```，以便内部的```img-item```可以使用绝对定位来控制每一个的定位。


![img-item结构图](http://7xrunf.com1.z0.glb.clouddn.com/img-item%E7%BB%93%E6%9E%84.png)


```img-item```内部包含一个```img```标签和一个```class```为```img-modal```的遮照层，```img```标签当然是用来展现图片的咯，当图片移动到左右两侧时会暗淡下来，```img-modal```就是用来实现这个效果的，当```img-item```在正面的时候```img-modal```的颜色为透明，而```img-item```移动到侧面时给```img-modal```的背景色设置为```rgba(0, 0, 0, 0.3)```。



内部结构说完了，再来说说布局，因为效果图上的三个图片之间是互相重叠的，并且最中间的是在最上层，所以我在这里没有采用设置```margin-left```为负值的方法，而采用了绝对定位，并且赋予不同的```z-index```。结构图中5个```img-item```都有一个涉及到```z-index、opacity、left、transform```的基本样式，然后根据位置的不同，又有```left-img-item、prev-img-item、main-img-item、next-img-item、right-img-item```等```class```添加到相应的```img-item```上，s使用不同属性来体现出各个```img-item```的差异，不同的```img-item```使用不同的```left```来作一个排列，而最左的```img-item```和最右的```img-item```透明度为0，中间三个```img-item```透明度为1，每一个```img-item```都设置```transform```中的```perspective、rotateY、translateZ```三个属性，```perspective```是控制视角的，```rotateY```是控制图片沿着Y轴旋转的，```translateZ```让图片产生大小的效果，推荐张鑫旭的[好吧，CSS3 3D transform变换，不过如此！](http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/)，具体参数不细说了，可以看样式文件，我这里也没有和网易云音乐中一样的标准，是在不停地微调中凑出来的。最后给图片设置个```transition:all .8s;```，这样在切换的时候就有过渡效果了。



至于```switch-list```没有什么需要特别的地方，就是个```ul```包着几个```li```，每个```li```修改下样式，成为灰色的小圆，然后有个选中的```li```加个```switch-item-active```，修改背景色。



接下来说下js部分，这里采用构造函数模式来封装。



```javascript


function VCarousel(opts) {

    this.wrap = opts.wrap || $('body');

    this.carousel = null;

    this.imgItems = null;

    this.switchItems = null;

    this.listSize = 0; //图片数量

    this.mainImgItem = null, //对应正中的img-item

    this.prevImgItem = null, //对应左边的img-item

    this.nextImgItem = null, //对应右边的img-item

    this.leftImgItem = null, //对应最左边的img-item，隐藏看不见

    this.rightImgItem = null, //对应最右边的img-item，隐藏看不见

    this.mainSwitchItem = null;

    this.init(opts.data);

}

VCarousel.prototype = {

    init:function(data){

        //初始化，根据数据添加dom节点，并给VCarousel中的属性赋值...

    },

    bindEvent: function(){

        //绑定事件，主要是点击每个图片时触发的事件和点击切换按钮时的事件...

    },

    clearStyle:function(){

        //清除样式，给几个特定位置的img-item清除特定样式...

    },

    addStyle: function(){

       //添加样式，给几个特定位置的img-item添加特定样式...

    },

    switchItem:function(){

        //切换img-item，也就是轮播中图片的方法...

    }

}

```


类```VCarousel```的基本属性中包含了几个不同位置的```img-item```以及图片数量和当前的切换按钮等，当new一个```VCarousel```的时候需要传入相关参数，挂载的元素和数据，这里使用了jQuery，所以传入的是一个jQuery对象，在init方法中会根据传入的数据拼接DOM字符串并添加到相应节点，当添加的DOM渲染完成后去计算出添加的图片数量，并选出几个特殊位置的```img-item```初始化相应的样式，然后绑定事件，主要是点击每个图片时触发的事件和点击切换按钮时的事件，当切换时，会去删除原有几个特定位置img-item的特定样式，然后根据点击的img-item去重新确定排列在特定位置的各个img-item，再赋予各个特定样式，此前在```css```中已经添加了过渡效果，从而呈现出一个过渡动画。





