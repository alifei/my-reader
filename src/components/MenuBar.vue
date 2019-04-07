<template>
    <div class='menu-bar'>
        <transition name='slide-up'>
            <div class="menu-wrapper" v-show="ifShow" :class="{'hide-box-shadow':
            ifSettingShow || !ifShow}">
                <div class="icon-wrapper">
                    <span class="icon-menu icon" @click="showSetting(3)"></span>
                </div>
                <div class="icon-wrapper">
                    <span class="icon-git-commit icon" @click="showSetting(2)"></span>
                </div>
                <div class="icon-wrapper">
                    <span class="icon-sun icon" @click="showSetting(1)"></span>
                </div>
                <div class="icon-wrapper">
                    <span class="icon-type icon" @click='showSetting(0)'></span>
                </div>
            </div>
        </transition>
        <transition name='slide-up'>
                <div class="setting-wrapper" v-show="ifSettingShow" >
                    <div class="setting-font-size" v-if="showTag===0">
                        <div class="preview-left" :style="{fontSize:fontSizeList[0].fontSize+'px'}">T</div>
                        <div class="select">
                            <div class="select-wrapper" v-for="(item,index) in fontSizeList" :key='index'
                            @click='setFontSize(item.fontSize)'>
                                <div class="line"></div>
                                <div class="point-wrapper">
                                    <div class="point" v-show="defaultFontSize===item.fontSize">
                                        <div class="small-point"></div>
                                    </div>
                                </div>
                                <div class="line"></div>
                            </div>
                        </div>
                        <div class="preview-right" :style="{fontSize:fontSizeList[7].fontSize+'px'}">T</div>
                    </div>
                    <div class="setting-theme" v-else-if="showTag===1">
                        <div class="setting-theme-item" v-for="(item,index) in themeList" :key="index" @click="setTheme(index)">
                            <div class="preview" :style="{background:item.style.body.background}"></div>
                        </div>
                    </div>
                    <div class="setting-progress" v-else-if="showTag===2">
                        <div class="setting-progress-wrapper">
                            <input type="range" max="100" min="0" step="1" class="progress-item" 
                            @change="onProgressChange($event.target.value)"
                            @input="onProgressInput($event.target.value)" 
                            :value="progress"
                            :disabled="!bookAvailable"
                            ref="progress">
                        </div>
                        <div class="text-wrapper">
                            <span>{{bookAvailable ? progress +'%' : '加载中…'}}</span>
                        </div>
                    </div>
                </div>
        </transition>
        <ContentView :ifShowContent="ifShowContent"
        v-show="ifShowContent"
        :navigation="navigation"
        :bookAvailable="bookAvailable"
        @jumpTo="jumpTo"></ContentView>
        <transition name="fade">
            <div class="content-mask" v-show="ifShowContent"
            @click="hideContent">
            </div>
        </transition>
    </div>
</template> 
<script>
import ContentView from "@/components/ContentView"
export default {
    name:'MenuBar',
    data(){
        return{
            ifSettingShow:false,
            showTag:0,
            progress:0,
            ifShowContent:false
        }
    },
    components:{
        ContentView
    },
    props:{
        ifShow:{
            type:Boolean,
            default:false
        },
        fontSizeList:Array,
        defaultFontSize:Number,
        themeList:Array,
        defaultTheme:Number,
        bookAvailable:{
            type:Boolean,
            default:false
        },
        navigation: Object
    },
    methods:{
        showSetting(tag){
            this.showTag=tag
            if(this.showTag===3){
                this.ifSettingShow=false
                this.ifShowContent=true
            }else{
                this.ifSettingShow=true
            }
        },
        hideSetting(){
            this.ifSettingShow=false
        },
        setFontSize(fontSize){
            this.$emit("setFontSize",fontSize)
        },
        setTheme(index){
            this.$emit("setTheme",index)
        },
        onProgressChange(progress) {
            this.$emit('onProgressChange', progress)
        },
        onProgressInput(progress){
            this.progress = progress
            this.$refs.progress.style.backgroundSize = `${this.progress}% 100%`
        },
        hideContent(){
            this.ifShowContent=false
        },
        jumpTo(target){
            this.$emit("jumpTo",target)
        }
    }
}
</script>
<style lang='scss' scoped>
@import '../assets/styles/global';
.menu-bar{
    .menu-wrapper{
        position:absolute;
        bottom:0;
        left:0;
        width:100%;
        height:px2rem(40);
        z-index:101;
        display:flex;
        background-color: white;
        box-shadow: 0 px2rem(-4) px2rem(4) rgba(0,0,0,.15);
        .icon-wrapper{
            flex:1;
            @include center;
        }
        .icon-git-commit{
            font-size:px2rem(27);
        }
    }
    .hide-box-shadow{
        box-shadow:none;
    }
    .setting-wrapper{
        position:absolute;
        bottom:px2rem(40);
        left:0;
        z-index:101;
        width:100%;
        height:px2rem(35);
        background-color: white;
        box-shadow: 0 px2rem(-4) px2rem(4) rgba(0,0,0,.15);
        .setting-font-size{
            display:flex;
            height:100%;
            .preview-left{
                flex:0 0 px2rem(40);
                @include center;
                padding-left:5%;
            }
            .preview-right{
                flex:0 0 px2rem(40);
                @include center;
                padding-right:5%;
            }
            .select{
                display:flex;
                flex:1;
                .select-wrapper{
                    flex:1;
                    @include center;
                    align-items:center;                        
                    .line{
                        flex:1;
                        height:0;
                        border-top:px2rem(1) solid #ccc;
                    } 
                    .point-wrapper{
                        position:relative;
                        flex:0 0 0;
                        height:px2rem(7);
                        width:0;
                        border-left:px2rem(1) solid #ccc;
                        .point{
                            position:absolute;
                            top:px2rem(-8);
                            left: px2rem(-8);
                            width:px2rem(20);
                            height:px2rem(20);
                            border-radius:50%;
                            background:white;
                            box-shadow:0 px2rem(4) px2rem(4) rgba(0,0,0,.15);
                            @include center;
                            .small-point{
                                width:px2rem(5);
                                height:px2rem(5);
                                border-radius:50%;
                                background:black;
                            }
                        }
                    }
                }
                :first-child{
                    .line{
                        &:first-child{
                            border-top:none;
                        }
                    }
                }
                :last-child{
                    .line{
                        &:last-child{
                            border-top:none;
                        }
                    }
                }
            }
        }  
        .setting-theme{
            display:flex;
            height:100%;
            .setting-theme-item{
                flex:1;
                display:flex;
                flex-direction: column;
                padding:px2rem(2);
                .preview{
                    flex:1;
                    border:px2rem(1) solid #8c8c8c;
                    border-radius:px2rem(3);
                }
            }
        }  
        .setting-progress{
            position:relative;
            width:100%;
            height:100%;
            .setting-progress-wrapper{
                width:100%;
                height:100%;
                @include center;
                padding:0 px2rem(30);
                box-sizing:border-box;
                .progress-item{
                    width: 100%;
                    -webkit-appearance: none;
                    height: px2rem(2);
                    background: -webkit-linear-gradient(#999, #999) no-repeat, #ddd;
                    background-size: 0 100%;
                    
                }
                :focus {
                    outline: none;
                }
                ::-webkit-slider-thumb {
                    -webkit-appearance: none;
                    height: px2rem(40);
                    width: px2rem(40);
                    border-radius: 50%;
                    background-color: white;
                    box-shadow: 0 4px 4px 0 rgba($color: #000, $alpha: .15);
                    border: px2rem(1) solid #ddd;
                }
            }
            .text-wrapper {
                position: absolute;
                left: 0;
                bottom: 0;
                width: 100%;
                color: #333;
                font-size: px2rem(12);
                text-align: center;
            }
        }
    }
    .content-mask{
        position:absolute;
        top:0;
        left:0;
        z-index:101;
        display:flex;
        width:100%;
        height:100%;
        background:rgba(51,51,51,.8);
    }
}

</style>
