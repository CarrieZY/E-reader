<template>
<transition name="slide-down">
        <div class="title-wrapper" v-show="navhide">
            <div class="left">
                <svg class="icon" aria-hidden="true">
                    <use xlink:href="#icon-fanhui"></use>
                </svg>
            </div>
            <div class="right">
                <div class="icon-wrapper">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-gouwuche"></use>
                    </svg>
                </div>
                <div class="icon-wrapper">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-geren"></use>
                    </svg>
                </div>
                <div class="ion-wrapper">
                    <svg class="icon" aria-hidden="true">
                        <use xlink:href="#icon-menu2"></use>
                    </svg>
                </div>
            </div>
        </div>
</transition>
</template>

<script>
export default {
    name:'titlebar',
    props:{
        navhide:{
            type:Boolean,
            default:false
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/styles/global';
    .title-wrapper{
        position:absolute;
        top:0;
        left:0;
        display: flex;
        z-index:101;
        width:100%;
        height:px2rem(48);
        background: #ffffff;
        box-shadow: 0 px2rem(8) px2rem(8) rgba(0,0,0,.15);
        .left{
            flex: 0 0 px2rem(60);
            @include center;
            }
        .right{
            flex:1;
            display:flex;
            justify-content: flex-end;
            .icon-wrapper{
                flex:0 0 px2rem(40);
                @include center;
            }
        }
    }

</style>
