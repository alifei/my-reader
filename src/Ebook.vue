<template>
    <div class='ebook'> 
        <TitleBar :ifShow='ifShow'></TitleBar>
        <div class="read-wrapper">
            <div id="read"></div>
            <div class="mask">
                <div class="left" @click='prevPage'></div>
                <div class="center" @click="toggleIfShow"></div>
                <div class="right" @click='nextPage'></div>
            </div>
        </div>
        <MenuBar :ifShow='ifShow' 
        :fontSizeList="fontSizeList" 
        :defaultFontSize="defaultFontSize" 
        @setFontSize="setFontSize" 
        :themeList="themeList"
        :defaultTheme="defaultTheme"
        @setTheme="setTheme"
        :bookAvailable="bookAvailable"
        @onProgressChange="onProgressChange"
        @jumpTo="jumpTo"
        :navigation="navigation"
        ref="menuBar"></MenuBar>
    </div>
</template>
<script>
import Epub from 'epubjs'
import TitleBar from '@/components/TitleBar'
import MenuBar from '@/components/MenuBar'
global.ePub=Epub
const DOWNLOAD_URL='/static/学习正则表达式.epub'
export default {
    name:'Ebook',
    data(){
        return{
            ifShow:false,
            fontSizeList:[
                {fontSize:12},
                {fontSize:14},
                {fontSize:16},
                {fontSize:18},
                {fontSize:20},
                {fontSize:22},
                {fontSize:24},
                {fontSize:26},
            ],
            defaultFontSize: 16,
            themeList:[
                {
                    name:"default",
                    style:{
                        body:{
                            "color":"#000000",
                            "background":"#ffffff"
                        }
                    }
                },
                {
                    name:"eye",
                    style:{
                        body:{
                            "color":"#000000",
                            "background":"#ceeaba"
                        }
                    }
                },
                {
                    name:"night",
                    style:{
                        body:{
                            "color":"#ffffff",
                            "background":"#000000"
                        }
                    }
                },
                {
                    name:"grey",
                    style:{
                        body:{
                            "color":"#ffffff",
                            "background":"#8a8a5c"
                        }
                    }
                },
                {
                    name:"brown",
                    style:{
                        body:{
                            "color":"#fff7e6",
                            "background":"#664400"
                        }
                    }
                }
            ],
            defaultTheme:0,
            bookAvailable:false,
            navigation:null
        }
    },
    components:{
        TitleBar,
        MenuBar
    },
    methods:{
        showEpub(){
            this.book=new Epub(DOWNLOAD_URL)
            this.rendition=this.book.renderTo('read',{
                width:window.innerWidth,
                height:window.innerHeight
            })
            this.rendition.display()
            this.themes=this.rendition.themes
            this.setFontSize(this.defaultFontSize)
            this.registerTheme()
            this.setTheme(this.defaultTheme)
            this.book.ready.then(()=>{
                this.navigation=this.book.navigation
                return this.book.locations.generate()
            }).then(result=>{
                this.locations=this.book.locations
                this.bookAvailable=true
            })
        },
        prevPage(){
            if(this.rendition){
                this.rendition.prev()
            }
        },
        nextPage(){
            if(this.rendition){
                this.rendition.next()
            }
        },
        toggleIfShow(){
            this.ifShow=!this.ifShow
            if(!this.ifShow){
                this.$refs.menuBar.hideSetting()
            }
        },
        setFontSize(fontSize){
            this.defaultFontSize=fontSize
            if(this.themes){
                this.themes.fontSize(fontSize+"px")
            }
        },
        registerTheme(){
            this.themeList.forEach(theme=>{
                this.themes.register(theme.name,theme.style)
            })
        },
        setTheme(index){
            this.themes.select(this.themeList[index].name)
            this.defaultTheme=index
        },
        onProgressChange(progress){
            const percentage=progress/100
            const location=percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
            this.rendition.display(location)
        },
        jumpTo(href){
            this.rendition.display(href)
            this.hide()
        },
        hide(){
            this.ifSettingShow=false
            this.$refs.menuBar.hideSetting()
            this.$refs.menuBar.hideContent()
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
}
.read-wrapper{
    .mask{
        position:absolute;
        top:0;
        left:0;
        z-index:100;
        width:100%;
        height:100%;
        display:flex;
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
</style>


 
