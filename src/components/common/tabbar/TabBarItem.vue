<template>
    <div class="tab-bar-item" @click="itemClick">
        <div v-if="!isActive"><slot name="item-icon"></slot></div>
        <div v-else><slot name="item-icon-active"></slot></div>
        <div :style="activeStyle"><slot name="item-text"></slot></div>
    </div>
</template>

<script>
    export default {
        name: "TabBarItem",
        props:{
            path:String,
            activeColor:{
                type:String,
                default:'red'
            }
        },
        data(){
            return{
                // isActive:true
            }
        },
        computed:{
            isActive(){
                return this.$route.path.indexOf(this.path) !== -1
            },
            activeStyle(){
                return this.isActive ? {color: this.activeColor}:{}
            }
        },
        methods:{
            itemClick() {
                // this.$router.replace(this.path)

                // 在升级了Vue-Router版本到到3.1.0及以上之后，页面在跳转路由控制台会报Uncaught (in promise)的问题
                // 在3.1.0版本里面新增功能：push和replace方法会返回一个promise, 你可能在控制台看到未捕获的异常
                //在调用方法的时候用catch捕获异常
                this.$router.replace(this.path).catch(e=>{
                    // console.log(e)
                })
            }
        }
    }
</script>

<style scoped lang="stylus">
    .tab-bar-item
        flex 1
        text-align: center
        height 49px
        font-size 14px
    .tab-bar-item img
        width 24px
        height 24px
        margin-top 3px
        vertical-align middle
        margin-bottom 2px
</style>