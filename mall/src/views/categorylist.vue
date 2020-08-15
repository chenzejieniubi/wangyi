<template>
    <div id="categoryList">
        <h1>产品分类列表</h1>
        <van-tabs v-model="tabActive">
            <van-tab v-for="(item,index) in clist" :key="index" :title="item.name">
                <h3>{{item.name}}</h3>
                <p>{{item.front_name}}</p>

                <!-- 怎么拿到产品和内容呢？
                1.看api.js文件
                    GoodsList: ApiRootUrl + 'goods/list',  //获得商品列表
                    //{ brandId: that.data.id, page: that.data.page, size: that.data.size}
                    想要获取到商品，就需要传3个参数，商品id、想要获取第几页的数据、获取商品数量。
                2.监听tabActive，因为这个是跟索引值绑定的
                设置3个参数，id page size
                然后获取，使用axios
                let res = await axios.get(this.$root.api.GoodsList,{params:{id,page,size}})
                使用这个确实能获取到数据，为什么呢，axios回去重新看。

                将其封装成方法。getlist（）


                因为在tab的循环下，商品需要循环显示，于是就一次性全部请求，就不需要去监听。

                箭头函数需要学一下。


                3.tab标签页有点击事件
                
                 -->
                <div v-if="item.plist">
                <van-grid :border="true" :column-num="2">
                <van-grid-item v-for="(item1,index1) in item.plist.data" :key="index1" :to="'/product/'+item1.id"> 
                    <van-image
                    width="10rem"
                    height="10rem"
                    fit="contain"
                    :src="item1.list_pic_url"
                    />
                    <h4 class="vant-ellipsis">{{item1.name}}</h4>
                    <p class="price">￥{{item1.retail_price}}</p>
                </van-grid-item>
                
                </van-grid>
                </div>

            </van-tab>
            
        </van-tabs>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    props:['id'],
    data(){
        return{
            tabActive:0,
            clist:[],
        }
    },
    async created(){
        // console.log(this.id)
        // console.log(this.$root.api)
        let res = await axios.get(this.$root.api.GoodsCategory,{params:{id:this.id}})
        console.log(res.data)
        this.clist =res.data.data.brotherCategory
        let id =this.clist[0].id;
        // this.getlist(id,1)
        this.clist.forEach(async (item,index) => {
            item.plist = await this.getlist(item.id,1)
            console.log(item.plist)
            this.$forceUpdate()
        });

    },
    // watch:{
    //     tabActive:async function(index){
    //         console.log(index)
    //         let id = this.clist[index].id
    //         let page = 1;
    //         let size = 20;
    //        // let res = await axios.get(this.$root.api.GoodsList,{params:{categoryID:id,page,size}})
    //         this.getlist(id,page);
    //     }
    // },
    methods:{
        async getlist(cid,page){
            let res = await axios.get(`${this.$root.api.GoodsList}?categoryId=${cid}&page=${page}&size=20`)
            console.log(res)
            return res.data.data
        }
    },
}
</script>

<style lang="less">
    #categoryList{
        .van-ellipsis{
            width: 100%;
            font-size: 14px;
            font-weight: 500;
            padding: 0 10px;

        }
        .van-grid-item{
            overflow: hidden;
            box-sizing: border-box;
        }
        .price{
            color:red
        }
    }
</style>