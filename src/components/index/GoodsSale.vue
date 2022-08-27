<template>
  <div class="goods-sale">
    <div  v-for="(item,i) in postItems.length" :key="i">
      <poster :postItem="postItems[i]"></poster>
      <goods-box :goodsItem="goodsItems[i]" ></goods-box>  
    </div>
  </div>
</template>

<script>
import Poster from './Poster'   // 分类推荐
import GoodsBox from './GoodsBox'  // 分类推荐对应的推荐商品

export default {
  data () {
    return {
      postItems:[],
      goodsItems:[]
    }
  },

   // 生命周期钩子函数
  mounted() {
    this.init();
  },
  methods:{
    init(){
      // 获取分类推荐、分类推荐对应的推荐商品
        this.axios.get("/home/goods_sale").then((res) => { 
            for (let index = 0; index < res.length; index++) {
              const cate = res[index];
              this.postItems.push({
                src:cate.pic,
                url:cate.url
              });

              let goods=[];
              let productList=cate.productList;
              for (let j = 0; j < productList.length; j++) {
                const product = productList[j];//用product接受收一下
                goods.push({//通过push添加进去
                  value:product.name,//商品名字
                  desc:product.subTitle,//副标题
                  sub:product.sub==0?true:false,//起字
                  newPrice:product.price,//折扣价
                  oldPrice:product.price<product.originalPrice?product.originalPrice:null,//市场价
                  src:product.pic,//图片
                  url:"/#/detail/"+product.id//跳转到商品详情
                })
              }


              this.goodsItems.push({
                boxTitle:cate.categoryName ,
                url: '/#/searchResult/'+cate.categoryName,
                listData:{
                  goods:goods
                }
              })

            }
        });
    }
  },
  components: {
    'Poster': Poster,
    'GoodsBox': GoodsBox
  }
}
</script>

<style lang="less">
  .goods-sale {
      position: relative;
      
    text-align: center;
      width: 1226px;
      height: auto;
      margin: 0 auto;
      display: flex;
      flex-direction: column;
  }
  .goods-sale *{    box-sizing: border-box;}
</style>