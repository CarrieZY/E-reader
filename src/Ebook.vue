<template>
    <div class="ebook">
        <titlebar :navhide="navhide"></titlebar>
        <div class="read-wrapper">
            <div id="read"></div>
            <div class="mask">
                <div class="left" @click="prevPage"></div>
                <div class="center" @click="navShow"></div>
                <div class="right" @click="nextPage"></div>
            </div>
        </div>
        <menubar :navhide="navhide" 
        :fontSizelist="fontSizelist"
         :defaultFontSize="defaultFontSize" ref="menubar"
         :themeList="themeList"  :defaultTheme="defaultTheme"
         @setTheme="setTheme"
        @setFontSize="setFontSize"
        :bookAvailable="bookAvailable"
        @onProgressChange="onProgressChange"
        :navigation="navigation"
        @jumpTo="jumpTo"
        ></menubar>
    </div>
</template>

<script>
import titlebar from './components/titlebar'
import menubar from './components/menubar'
import Epub from 'epubjs'
const DOWNLOAD_URL='/static/2018_Book_AgileProcessesInSoftwareEngine.epub'
global.epbu=Epub
export default {
    name:'book',
    components:{
        titlebar,
        menubar
    },
    data(){
        return {
            navhide:false,
            fontSizelist:[
                {fontSize:12},
                {fontSize:14},
                {fontSize:16},
                {fontSize:18},
                {fontSize:20},
                {fontSize:22},
                {fontSize:24}
            ],
            defaultFontSize:16,
            themeList:[
                {name:'default',style:{
                    body:{
                        'color':'#000','background':'#fff'
                    }
                }},
                {name:'eye',style:{
                    body:{
                        'color':'#000','background':'#ceeaba'
                    }
                }},
                {name:'nigth',style:{
                    body:{
                        'color':'#fff','background':'#000'
                    }
                }},
                {name:'gold',style:{
                    body:{
                        'color':'#000','background':'rgb(241,236,266)'
                    }
                }}
            ],
            defaultTheme:0,
            bookAvailable: false,
            navigation: {}
        } 
    },
    methods:{
        //jump to href
        jumpTo(href) {
            this.rendition.display(href);
            this.hideTtleAndMenu(); 
        },
        hideTtleAndMenu() {
            //hide titleBar and menuBar
            this.ifTitleAndMenuShow = false;
            //hide setting
            this.$refs.menuBar.hideSetting();
            //hide content
            this.$refs.menuBar.hideContent();
        },
        //progress : 0-100
        onProgressChange(progress){
            const percentage = progress / 100;
            const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0;
            this.rendition.display(location);
        },
        setTheme(index){
            this.themes.select(this.themeList[index].name)
            this.defaultTheme
        },
        registerTheme(){
            this.themeList.forEach(theme =>{
                this.themes.register(theme.name,theme.style)
            })
        },
        setFontSize(fontSize){
            this.defaultFontSize = fontSize
            if(this.themes){
                this.themes.fontSize(fontSize+'px')
            }
        },
        navShow(){
            this.navhide=!this.navhide
            if(!this.navhide){
                this.$refs.menubar.hideset()
            }
        },
        prevPage(){
            // rendition.prev
            if(this.rendition){
                this.rendition.prev()
            }
        },
        nextPage(){
            //rendition.next
            if(this.rendition){
                this.rendition.next()
            }
        },
        // 电子书的解析和渲烂 
        showEpub(){
            // 生成book对象
            this.book=new Epub(DOWNLOAD_URL)
            console.log(this.book)
            //生成rendition 通过book对象的renderTo方法生成的
            this.rendition=this.book.renderTo('read',{
                width:window.innerWidth,
                height:window.innerHeight
            })
            //生成rendition.display渲烂电子书
            this.rendition.display()
            //获取到themes对象
            this.themes=this.rendition.themes
            //设置默认字体
            this.setFontSize(this.defaultFontSize)
            //实现主题切换this.themes.register(name,style)
            //通过主题名称来切换this.themes.select(name)
            this.registerTheme()
            // this.themes.select('nigth')
            this.setTheme(this.defaultTheme)
            // console.log(this.book.loacation)
            this.book.ready.then(() =>{
                this.navigation = this.book.navigation
                return this.book.locations.generate()
            }).then(result =>{
                this.locations =this.book.locations
            })

        } 
    },
    mounted(){
        this.showEpub()
    }
    
}
</script>

<style lang="scss" scoped>
@import 'assets/styles/global';
.ebook{
    position:relative;
    .read-wrapper{
        .mask{
            position:absolute;
            top:0;
            left:0;
            z-index:100;
            width:100%;
            height:100%;
            display: flex;
            .left{
                flex:0 0 px2rem(100);
            }
            .center{
                flex:1;
            }
            .right{
                flex:0 0 px2rem(100);
            }
        }
    }
    .menu-wrapper{
        position:absolute;
        bottom:0;
        left:0;
        width:100%;
        z-index:101;
        background: #ffffff;
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,.15);
        display: flex;
        .icon-wrapper{
            flex: 1;
           @include center;
            #icon-taiyang{
                font-size: px2rem(24);
            }
        }
    }
}   

</style>

