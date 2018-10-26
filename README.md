# reader

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```
安装依赖：
npm install node-sass sass-loader --save-dev

阅读器引擎：npm install  epubjs --save



*rem的配置：
rem的值相当于根元素的fony-size值的倍数
1rem=根元素font-size
2rem=根元素font-size*2

随着屏幕的宽的变化  rem的值也会跟着变化
DOMCententLoaded事件动态的设置根元素font-size
HTML.style.font-size=wwindow.innerWidth/10+'px'


引入reset.scss和globa.scss
globa.scss规定整个站点的公共样式，公共方法和参数等
实现px2rem的方法，将px转化为rem
font-size的配置：
@import 'reset';
//  1rem = fontSize
//  1px =(1/fontSize)rem 

$font-size:37.2;

@function px2rem($px){
    @return ($px/ $font-size)+rem ;
}

@mixin center() {
    display: flex;
    justify-content: center;
    align-items: center;
}