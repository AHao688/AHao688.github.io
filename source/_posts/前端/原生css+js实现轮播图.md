---
title: '原生css+js实现轮播图'
date: '2022-7-27 20:40:00'
cover: https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/cover3.png
categories: 
- 前端
tags:
- 前端
- 轮播图
- JavaScript
- CSS
---

本篇文章给大家介绍一下使用原生CSS、JavaScript实现轮播图的方法。我总结了一下，把轮播图的实现分为两种方法：1、用图片移动实现轮播图；2、用opacity实现轮播图 （当然原生代码实现轮播图的方法有很多种，这里呢给大家分享一下我认为比较实用的方法）

# 用图片移动实现轮播图
## 思路
先生成一个div盒子作为展示区，展示区的宽度为1张图片的宽度，然后把所有的图片水平平铺，接着让超出展示区的图片隐藏。最后通过设置定时器，改变 left 来拉动图片使展示区显示不同的图片
## 代码
### HTML部分
```html
    <div id="wrap">
        <div id="lunbo">
            <!-- 轮播图图片 -->
            <ul class="lunbo_list">
                <li class="item"><img src="./img/bg1.jpg"></li>
                <li class="item"><img src="./img/bg2.jpg"></li>
                <li class="item"><img src="./img/bg3.jpg"></li>
            </ul>
            <!-- 轮播图下方的小圆点 -->
            <ul class="min-point">
                  <li class="min active"></li>
                  <li class="min"></li>
                  <li class="min"></li>
            </ul>
            <!-- 左箭头图片 -->
            <img src="./img/banner-left.png" class="btn" id="gopre">
            <!-- 右箭头图片 -->
            <img src="./img/banner-right.png" class="btn" id="gonext">
        </div>
    </div>
```
### CSS部分
```css
*{
    margin: 0;
    padding: 0;
 } /*清除默认margin padding样式的影响 */
#wrap{
    width: 700px;
    height: 460px; 
    margin: 55px auto;
} /* 给盒子设置宽高并水平居中 */
#lunbo{	
    height: 100%;
	position: relative;
	overflow-x: hidden;
}/* 设置盒子为相对定位,让盒子宽度超出的部分隐藏 */
/* 以下使图片在水平方向平铺 */
.lunbo_list{
	width: 300%;
	height: 100%;
    list-style: none;
    display: flex;
	position: absolute;
	left: 0;
	transition: .2s;
}
.item{
	width: 100%;
}
.item img{
    width: 100%;
    height: 450px;
}

/* 使小圆点定位到轮播图下方 */
.min-point{
    padding-left: 0;
    list-style: none;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 35px;
}
.min{
    width: 10px;
    height: 10px;
    background-color: #8B888B;
    border-radius: 50%;
    float: left;
    margin-right: 14px;
	cursor: pointer; /* 鼠标经过或点击时 鼠标光标变为小手 */
    
}
.min.active{
	background-color: #fff;
} /* 设置小圆点点击后的样式，这里点击后背景颜色变为白色 */

.btn{
    width: 50px;
    height: 100px;
    z-index: 10;
    cursor: pointer;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    display: none;
} /* 设置左右箭头的样式，并隐藏 */

#gopre{
    left: 0;
}
#gonext{
    right: 0;
}
```
### JS部分
```JavaScript
let min=document.querySelectorAll('.min');  //获取 所有class为min的元素
let images=document.querySelector('.lunbo_list'); //获取class为lunbo_list的元素（第一个元素）
let index=0; // 定时器时间
let time; // 轮播图图片和小圆点的下标

let lunbo_div=document.getElementById('lunbo'); // 获取id为 lunbo 的元素
let btns=document.querySelectorAll('.btn'); // 获取 所有class为btn的元素

function position(){ //实现轮播的关键函数
	images.style.left = (index * -100) + "%" //使每张图片向左移动100%，即一张图片的距离
	minActive() //调用小圆点改变的函数,因为图片和小圆点要同时移动
}

function add(){ //向右移动一张图片
	if(index>=min.length-1){
		index=0
	}else{
		index++
	}
	// 假如一共是3张图片，移动到第3张时再向右移动，这时就要移动第1张，所以需要做判断当前是第几张图片
}
function desc(){ //向左移动一张图片
	if(index<1){
		index=min.length-1
	}else{
		index--
	}
}

function minActive() { //小圆点改变的函数
    for (var j = 0; j < min.length; j++) {
        if(j==index){
			min[j].className = 'min active' //通过为元素添加新的类名active，使小圆点改变样式（active已在css中设置过）
		}else{
			min[j].className = 'min'
		}
    }
}

function timer(){
	time=setInterval(()=>{
		index++
		desc()
		add()
		position()
	},2000) //设置定时器 2000为2000毫秒，即2秒后自动轮播
}

for(let i=0;i<min.length;i++){
	min[i].addEventListener('click',()=>{ //为小圆点设置点击事件
		index=i
		position()
		clearInterval(time) // 清除定时器
		timer()
	})
}
timer() //开启定时器


// 轮播左右箭头根据鼠标的移入、移出 来显示、隐藏图标
lunbo_div.addEventListener("mouseover",()=>{
	btns[0].style.display = "block"
	btns[1].style.display = "block"
})

lunbo_div.addEventListener("mouseout",()=>{
	btns[0].style.display = "none"
	btns[1].style.display = "none"
})


// 以下是点击是切换上一张 和 切换下一张 此处要考虑定时器的影响
btns[0].addEventListener("click",()=>{
	clearInterval(time)
	desc()
	position()
	timer()
})

btns[1].addEventListener("click",()=>{
	clearInterval(time)
	add()
	position()
	timer()
})
```
## 效果
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/lunbo1.jpg)
[点我查看完整效果](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/lunbo1.mp4)
# 用opacity实现轮播图
## 思路
先生成一个div盒子作为展示区，展示区的宽度为1张图片的宽度，然后设置所有图片的position为absolute，把所有图片定位到展示区堆叠起来，最后通过opacity将图片设为透明，把要展示的图片的设置为不透明，再配合定时器，从而使展示区显示不同的图片

## 代码
### HTML部分
```html
    <div id="lunbo">
        <!-- 轮播图图片 -->
        <ul class="list">
            <li class="item active"><img src="img/bg1.jpg"></li>
            <li class="item"><img src="img/bg2.jpg"></li>
            <li class="item"><img src="img/bg3.jpg"></li>
        </ul>
        <!-- 轮播图下方的小圆点 -->
        <ul class="pointlist">
            <li class="point active" data-index="0"></li>
            <li class="point" data-index="1"></li>
            <li class="point" data-index="2"></li>
        </ul>
        <!-- 左箭头图片 -->
        <img src="./img/banner-left.png" class="btn" id="gopre">
        <!-- 右箭头图片 -->
        <img src="./img/banner-right.png" class="btn" id="gonext">
    </div>
```
### CSS部分
```css
*{
    margin: 0;
    padding: 0;
} /*清除默认margin padding样式的影响 */
#lunbo{
    width: 700px;
    height: 460px;
    margin: 55px auto;
    position: relative;
} /* 给盒子设置宽高并水平居中 */
.list{
    width: 700px;
    height: 100%;
    list-style: none;
    position: relative;
    padding-left: 0;
} /* 设置ul无序列表为相对定位,使ul列表前面的小圆点消失*/
.item{
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 0;
    transition: all .8s;
} /* 设置每个li的位置，opacity让图片暂时隐藏 */
.item img{
	width: 700px;
	height: 450px;
}
.item.active{
    opacity: 1;
    /* z-index: 10; */
} 
/* 让li标签显示，即图片框的一张图片显示 
再通过js更改class类名即可实现自动轮播*/
.btn{
    width: 50px;
    height: 100px;
    z-index: 15;
    cursor: pointer;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    display: none;
} /* 设置左右箭头的样式，并隐藏 */

#gopre{
    left: 0;
}
#gonext{
    right: 0;
}

/* 使小圆点定位到轮播图下方 */
.pointlist{
    padding-left: 0;
    list-style: none;
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 35px;
    z-index: 100;
}
.point{
    width: 10px;
    height: 10px;
    background-color: #8B888B;
    border-radius: 50%;
    float: left;
    margin-right: 14px;
	cursor: pointer; /* 鼠标经过或点击时 鼠标光标变为小手 */
}
.point.active{
	background-color: #fff;
} /* 设置小圆点点击后的样式，这里点击后背景颜色变为白色 */

```
### JS部分
```JavaScript
let items = document.getElementsByClassName('item'); // 获取所有class为item的元素
let points = document.getElementsByClassName('point'); // 获取所有class为point的元素
let goPreBtn = document.getElementById('gopre'); // 获取id为gopre的元素
let goNextBtn = document.getElementById('gonext'); // 获取id为gonext的元素
let lunbo_div = document.getElementById('lunbo'); // 获取id为lunbo的元素

let time = 0; // 定时器时间
let index = 0; // 轮播图图片和小圆点的下标
function clearActive() {
    for (var i = 0; i < items.length; i++) {
        items[i].className = "item";
    }
    for (var i = 0; i < points.length; i++) {
        points[i].className = "point";
    }
}

function goIndex() { // 实现轮播的函数
    clearActive();
    points[index].className = 'point active';
    items[index].className = 'item active';
}

function next() { //切换下一张图片
    if (index < 2) {
        index++;
    } else {
        index = 0;
    }
    goIndex();
    // 一共是3张图片，移动到第3张时再向右移动，这时就要移动第1张，所以需要做判断当前是第几张图片
}
function pre() { // 切换上一张图片
    if (index == 0) {
        index = 2;
    } else {
        index--;
    }
    goIndex();
}

// 点击时切换下一张
goNextBtn.addEventListener('click', function () {
    next();
})

// 点击时切换上一张
goPreBtn.addEventListener('click', function () {
    pre();
})

for (var i = 0; i < points.length; i++) {
    points[i].addEventListener('click', function () {
        var pointIndex = this.getAttribute('data-index'); // 获取当前元素data-index的属性值
        index = pointIndex;
        goIndex();
        time = 0; //重新设置定时器时间
    })
}

setInterval(function () {
    time++;
    if (time == 20) {
        next();
        time = 0;
    }
}, 100); // 设置定时器 2000为2500毫秒，即2秒后自动轮播


// 轮播左右箭头根据鼠标的移入、移出 来显示、隐藏图标
lunbo_div.addEventListener("mouseover",()=>{
	goPreBtn.style.display = "block"
	goNextBtn.style.display = "block"
})

lunbo_div.addEventListener("mouseout",()=>{
	goPreBtn.style.display = "none"
	goNextBtn.style.display = "none"
})

```
## 效果
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/lunbo2.jpg)
[点我查看完整效果](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/lunbo2.mp4)

# 总结
使用原生css、js实现轮播图，最主要的是理解轮播图的主体思想，即 图片1 ——> 图片2 ——> 图片3 ——> 图片1 这个不断循环的过程，这样我们就很容易想到使用js的定时器来实现自动轮播。然后想办法让展示区每隔几秒展示不同图片，整个轮播图就完成了。
本文源码获取 ——> [点我获取源码](https://wwp.lanzouq.com/iWhuj08mw3pg)