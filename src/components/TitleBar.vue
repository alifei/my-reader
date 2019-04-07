<template>
    
        <transition name="slide-down">
            <div class="title-wrapper" v-show="ifShow">
                <div class="left">
                    <span class="icon-chevron-left icon"></span>
                </div>
                <div class="right">
                    <div class="icon-wrapper">
                        <span class="icon-user icon"></span>
                    </div>
                    <div class="icon-wrapper">
                        <span class="icon-more-vertical icon"></span>
                    </div>
                </div>
            </div>
        </transition>
    
</template>
<script>
export default {
    name:'TitleBar',
    props:{
        ifShow:{
            type:Boolean,
            default:false
        } 
    }
}
</script>
<style lang='scss' scoped>
@import '../assets/styles/global';
.title-wrapper{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:px2rem(40);
    z-index:101;
    display:flex;
    background-color: white;
    box-shadow: 0 px2rem(4) px2rem(6) rgba(0,0,0,.15);
    .left{
        flex:0 0 px2rem(48);
        @include center;
        .icon-chevron-left{
            font-size:px2rem(30);
        }
    }
    .right{
        flex:1;
        display:flex;
        justify-content:flex-end;
        .icon-wrapper{
           flex:0 0 px2rem(40);
            @include center;

        }
    }
}
</style>