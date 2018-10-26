<template>
<div class="menubar">
    <transition name="slide-up">
        <div class="menu-wrapper" :class="{'hide-box-show':setshow || !navhide}" v-show="navhide">
            <div class="icon-wrapper">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-huaban" @click="showset(3)"></use>
                </svg>
            </div>
            <div class="icon-wrapper">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-xuanzeanniu" @click="showset(2)"></use>
                </svg>
            </div>
            <div class="icon-wrapper">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-taiyang" @click="showset(1)"></use>
                </svg>
            </div>
            <div class="icon-wrapper">
                <svg class="icon" aria-hidden="true" @click="showset(0)">
                    <use xlink:href="#icon-a"></use>
                </svg>
            </div>
        </div>
    </transition>
    <transition name="slide-up">
    <div class="setting-wrapper" v-show="setshow" >
        <div class="setting-fontsize" v-if="showtag === 0" >
            <div class="preview" :style="{fontSize:fontSizelist[0].fontSize+'px'}">A</div>
            <div class="select">
                <div class="selsct-wrapper" v-for="(item,index) in fontSizelist" :key="index" @click="setfontsize(item.fontSize)">
                    <div class="line"></div>
                    <div class="point-wrapper">
                        <div class="point" v-show="defaultFontSize === item.fontSize">
                            <div class="small-point"></div>
                        </div>
                    </div>
                    <div class="line"></div>
                </div>
            </div>   
            <div class="preview" :style="{fontSize:fontSizelist[fontSizelist.length-1].fontSize+'px'}">A</div>
        </div>
        <div class="setting-theme" v-else-if="showtag===1">
            <div class="setting-theme-item" v-for="(item,index) in themeList" :key="index" @click="setTheme(index)">
                <div class="prevew" :style="{background:item.style.body.background}" :class="{'no-border': item.style.body.background != '#fff'}"></div>
                <div class="text" :class="{'selected': index === defaultTheme}">{{item.name}}</div>
            </div>
        </div>
        <div class="setting-progress" v-else-if="showTag===2">
                <div class="progress-wrapper">
                    <input class="progress" 
                        type="range"
                        max="100"
                        min="0"
                        step="1"
                        @change="onProgressChange($event.target.value)"
                        @input="onProgressInput($event.target.value)"
                        :value="progress"
                        :disabled="!bookAvailable"
                        ref="progress">
                </div>
                <div class="text-wrapper">
                    <span>{{bookAvailable ? progress + '%' : 'Loading'}}</span>
                </div>
            </div>
        </div>
    </transition>
    <content-view :ifShowContent="ifShowContent"
                    v-show="ifShowContent"
                    :navigation="navigation"
                    :bookAvailable="bookAvailable"
                    @jumpTo="jumpTo"></content-view>
    <transition name="fade">
        <div class="content-mask"
            v-show="ifShowContent"
            @click="hideContent"></div>
    </transition>
</div>    
</template>

<script>
import ContentView from '@/components/ContentView'
export default {
    components: {
        ContentView
    },
    name:'menubar',
    props:{
        navhide:{
            type:Boolean,
            default:false
        },
        fontSizelist:{
            type:Array
        },
        defaultFontSize:{
            type:Number
        },
        themeList:Array,
        defaultTheme:Number,
        bookAvailable: Boolean,
        navigation: Object
    },
    data(){
        return{
            setshow:false,
            showtag:0,
            progress: 0,
            ifShowContent: false
        }
    },
    methods:{
        hideContent() {
                this.ifShowContent = false;
        },
        jumpTo(target) {
                this.$emit('jumpTo', target);
        },
        onProgressInput(progress) {
                this.progress = progress;
                this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
        },
        onProgressChange(progress) {
                this.$emit('onProgressChange', progress);
        },
        setTheme(index){
            this.$emit('setTheme',index)
        },
        setfontsize(fontSize){
            this.$emit('setFontSize',fontSize)
        },
        showset(tag){
            this.showtag=tag
            if(this.showtag === 3) {
                    this.setshow = false;
                    this.ifShowContent = true;
                } else {
                    this.setshow = true;
                }
        },
        hideset(){
            this.setshow=false
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global';
.menubar{    
    position: relative;
    .setting-wrapper{
        width:100%;
        position: absolute;
        bottom: px2rem(48);
        left:0;
        height: px2rem(60);
        z-index:100;
        background: #ffffff;
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,.15);
        .setting-fontsize{
            display: flex;
            height:100%;
            .preview{
                flex: 0 0 px2rem(40);
                @include center;
            }
            .select{
                display: flex;
                flex:1;
                .selsct-wrapper{
                    flex:1;
                    display: flex;
                    align-items: center;
                    &:first-child{
                        .line{
                            &:first-child{
                                border-top:none;
                            }
                        }
                    }
                    &:last-child{
                        .line{
                            &:last-child{
                                border-top:none;
                            }
                        }
                    }
                    .line{
                        flex:1;
                        height:0;
                        border-top:px2rem(1) solid #ccc;
                    }
                    .point-wrapper{
                        position:relative;
                        flex:0 0 0 ;
                        width:0;
                        height:px2rem(7);
                        border-left:px2rem(1) solid #ccc;
                        .point{
                            position:absolute;
                            top:px2rem(-8);
                            left:px2rem(-8);
                            width:px2rem(20);
                            height:px2rem(20);
                            border-radius: 50%;
                            background:#fff;
                            border-left:px2rem(1) solid #ccc;
                            box-shadow: 0 px2rem(4) px2rem(4) rgba(0,0,0,.15);
                            @include center;
                            .small-point{
                                width:px2rem(5);
                                height:px2rem(5);
                                background:#000;
                                border-radius: 50%;
                            }
                        }
                    }
                }
            }
            
        }
        .setting-theme{
            display: flex;
            height:100%;
            .setting-theme-item{
                flex:1;
                display: flex;
                flex-direction: column;
                padding: px2rem(5);
                box-sizing: border-box;
                .prevew{
                    flex: 1;
                    border: px2rem(1) solid #ccc;
                    box-sizing: border-box;
                    &.no-border {
                        border: none;
                    }
                }
                .text{
                    flex: 0 0 px2rem(20);
                    font-size:px2rem(14);
                    color:#ccc;
                    @include center;
                    &.selected {
                        color: #333;
                    }          
                 }
            }
        }
        .setting-progress {
            position: relative;
            width: 100%;
            height: 100%;
            .progress-wrapper {
                width: 100%;
                height: 100%;
                @include center;
                padding: 0 px2rem(30);
                box-sizing: border-box;
                .progress {
                    width: 100%;
                    /* cover default CSS */
                    -webkit-appearance: none;
                    height: px2rem(2);
                    background: -webkit-linear-gradient(#999, #999) no-repeat, #ddd;
                    background-size: 0 100%; 
                    &:focus {
                        outline: none;
                    }
                    &::-webkit-slider-thumb {
                        -webkit-appearance: none;
                        height: px2rem(20);
                        width: px2rem(20);
                        border-radius: 50%;
                        background: #fff;
                        border: px2rem(1) solid #ddd;
                        box-shadow: 0 4px 4px 0 rgba(0, 0, 0, .15);
                    }
                }
            }
            .text-wrapper {
                position: absolute;
                left: 0;
                bottom: 0;
                width: 100%;
                color: #333;
                span {
                    font-size: px2rem(14);
                    @include center;
                }
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
        height:px2rem(48);
        box-shadow: 0 px2rem(-8) px2rem(8) rgba(0,0,0,.15);
        display: flex;
        &.hide-box-show{
            box-shadow: none;
        }
        .icon-wrapper{
            flex: 1;
           @include center;
            #icon-taiyang{
                font-size: px2rem(24);
            }
        }
    }
    .content-mask {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 101;
        display: flex;
        width: 100%;
        height: 100%;
        background: rgba(51, 51, 51, .8);
    }
}    
</style>
