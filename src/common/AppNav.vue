<template>
    <nav  class="app-nav">
        <ul class="app-list">
            <li v-for="(item,index) in item_name" :key="index" 
                class="app-item ac" @click="iconSelect(index)">
                    <!-- :class="key:value" key为显示的class名称，value是否取用。此外key取变量时需加上[]加以区分 -->
                    <i class="iconfont app-iconf" :class="{[item_className[index]]:true,[item_hover[index]]:true}">
                    </i>
                    <span class="title" :class="{[item_hover[index]]:true}">{{item}}</span>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    name: 'AppNav',
    data(){
        return{
            item_name:['文章','游戏','图书','音乐'],
            item_className:['icon-yuedu','icon-youxi','icon-tushu','icon-music'],
            item_hover:['','','','']
        }
    },
    watch:{
        $route(to,from){
            //去除音乐底部栏选中状态
            this.item_name.map((item,index)=>{
                if('music' === this.transName(item)){
                    this.item_hover.splice(index,1,'')
                }
            })
            if(this.$route.params.comment === 'return'){
                this.item_hover.splice(0,1,'select')
            }
        }
    },
    methods:{
        iconSelect(index){
            let temp = this.item_name[index]
            //清空之前的.select
            this.item_hover.map((item,i)=>{
                if(item=="select"){
                    //vue对数组的数据检测有严格规定,与对象一样。详情看官网
                    //传送门 => https://cn.vuejs.org/v2/guide/list.html#%E6%95%B0%E7%BB%84%E6%9B%B4%E6%96%B0%E6%A3%80%E6%B5%8B
                    this.item_hover.splice(i,1,'')
                    return
                }
            })
            this.item_hover.splice(index,1,'select')
            //路由传值
            this.$router.push({name:this.transName(temp),params:{title:temp}})
        },
        transName(ChineseName){
            switch(ChineseName){
                case '文章':
                    return 'article'
                    break
                case '游戏':
                    return 'game'
                    break
                case '图书':
                    return 'book'
                    break
                case '音乐':
                    return 'music'
                    break
            }
        }
    },
    mounted(){
        //解决刷新页面后重置底部状态栏
        let index = this.item_name.findIndex(item=>{
            if(this.$route.name === this.transName(item)){
                return true
            }
        })
        this.item_hover.splice(index,1,'select')
    }
}
</script>

<style scoped>
.app-nav{
    width: 100%;
    position: absolute;
    bottom: 0;
    left: 0;
    height: 1.306667rem;
}
.app-list{
    height: 100%;
    display: flex;
    flex-direction: row;
}
.app-item{
    box-sizing: border-box;
    padding-top: .133333rem;
    height: 100%;
    flex: 1;
    text-align: center;
}
.app-iconf{
    font-size: .56rem;
}
.title{
    display: block;
    color: #0084B4;
    font-size: .373333rem;
}
.select{
    color: #f09199;
}
</style>
