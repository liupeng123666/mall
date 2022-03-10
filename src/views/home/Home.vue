<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">首页</div></nav-bar>
        <tab-control class="tab-control"
                     :titles="['流行','新款','精选']"
                     @tabClick="tabClick"
                     v-show="isTabFixed"
                     ref="tabControl1"/>

        <scroll class="content"
                ref="scroll"
                :probe-type="3"
                @scroll="contentScroll"
                :pullUpLoad="true"
                @pullingUp="loadMore">
            <home-swiper :banners="banners" @swiperImgLoad="swiperImgLoad"></home-swiper>
            <home-recommended :recommends="recommends"/>
            <home-feature/>
            <tab-control :titles="['流行','新款','精选']"
                         @tabClick="tabClick"
                         ref="tabControl2"/>
            <goods-list :goods="goods[currentType].list"/>
        </scroll>

        <back-top @click.native="topClick" v-show="isShowBackTop"/>
    </div>
</template>

<script>
    import NavBar from 'components/common/navbar/NavBar'
    import Scroll from 'components/common/scroll/Scroll'

    import TabControl from 'components/content/tabControl/TabControl'
    import GoodsList from 'components/content/goods/GoodsList'
    import BackTop from 'components/content/backTop/BackTop'

    import HomeSwiper from 'views/home/childComps/HomeSwiper'
    import HomeRecommended from 'views/home/childComps/HomeRecommended'
    import HomeFeature from 'views/home/childComps/HomeFeature'

    import { getHomeMultidata,getHomeGoods } from "api/home"
    import {debounce} from "utils/utils"

    export default {
        name: "Home",
        components:{
            NavBar,
            Scroll,

            TabControl,
            GoodsList,
            BackTop,

            HomeSwiper,
            HomeRecommended,
            HomeFeature
        },
        data(){
          return{
              banners:[],
              recommends:[],
              goods:{
                  'pop':{page:0,list:[]},
                  'new':{page:0,list:[]},
                  'sell':{page:0,list:[]}
              },
              currentType:'pop',
              isShowBackTop:false,
              tabOffsetTop:0,
              isTabFixed:false,
              saveY:0,
          }
        },
        activated() {
            this.$refs.scroll.scrollTo(0, this.saveY, 0)
            this.$refs.scroll.refresh()
        },
        deactivated() {
            this.saveY = this.$refs.scroll.getScrollY()
        },
        created() {
            this.getHomeMultidata()
            this.getHomeGoods('pop')
            this.getHomeGoods('new')
            this.getHomeGoods('sell')
        },
        mounted(){
            const refresh = debounce(this.$refs.scroll.refresh, 50)
            this.$bus.$on('itemImageLoad', () => {
                refresh()
            })
                // this.$bus.$on('itemImageLoad',()=>{
                //     this.$refs.scroll.refresh()
                // })
        },
        methods:{
            tabClick(index){
                // console.log(index)
                switch(index){
                    case 0:
                        this.currentType = 'pop'
                        break
                    case 1:
                        this.currentType = 'new'
                        break
                    case 2:
                        this.currentType = 'sell'
                        break
                }
                this.$refs.tabControl1.currentIndex = index
                this.$refs.tabControl2.currentIndex = index
            },
            topClick(){
                this.$refs.scroll.scrollTo(0,0)
            },
            contentScroll(position){
                this.isShowBackTop = (-position.y) > 1000
                this.isTabFixed = (-position.y) > this.tabOffsetTop
            },
            loadMore(){
                this.getHomeGoods(this.currentType)
            },
            swiperImgLoad(){
                this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
                console.log(this.tabOffsetTop);
            },

            getHomeMultidata() {
                getHomeMultidata().then(res => {
                    this.banners = res.data.banner.list;
                    this.recommends = res.data.recommend.list;
                })
            },
            getHomeGoods(type){
                const page = this.goods[type].page + 1
                getHomeGoods(type,page).then(res => {
                    this.goods[type].list.push(...res.data.list)
                    this.goods[type].page += 1

                    this.$refs.scroll.finishPullUp()
                })
            }
        }
    }
</script>

<style scoped lang="stylus">
    #home
        /*padding-top 44px*/

        height 100vh
        position relative

    .home-nav
        background-color #ff8198
        color #fff

        /*position sticky*/
        /*top 0*/
        /*position fixed*/
        /*left 0*/
        /*right 0*/
        /*top 0*/
        /*z-index 9*/

    .content
        overflow hidden
        position absolute
        top 44px
        bottom 49px
        left 0
        right 0


        /*height calc(100% - 93px)
        overflow hidden
        margin-top 44px*/

    .tab-control
        position relative
        /*position sticky*/
        /*top 44px*/

        /*position fixed*/
        /*left 0
        /*right 0*/
        /*top 44px*/
        z-index 9
</style>