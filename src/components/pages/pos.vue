<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="pos-order">
        <el-tabs type='card'>
          <el-tab-pane label='点餐'>
            <el-table :data='tableData' border>
              <el-table-column prop='goodsName' label='商品名称'></el-table-column>
              <el-table-column prop='count' label='数量'></el-table-column>
              <el-table-column prop='price' label='金额'></el-table-column>
              <el-table-column prop='pay' label='操作' fixed='right'>
                <template scope="scope">
                   <el-button type='text' size='small' @click="addOrderList(scope.row)">增加</el-button>
                   <el-button type='text' size='small' @click="delSingleGoods(scope.row)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalDiv">
              <small>数量：</small>{{totalCount}}  &nbsp;  <small>金额：</small>{{totalMoney}}元
            </div>
            <div class="btn-diancan">
              <el-button type='warning'>挂单</el-button>
              <el-button type='danger' @click="delAllGoods">删除</el-button>
              <el-button type='success' @click="checkout">结账</el-button>
            </div>
          </el-tab-pane>

          <el-tab-pane label='购物车'>
            购物车
          </el-tab-pane>

          <el-tab-pane label='外卖'>
            外卖
          </el-tab-pane>

        </el-tabs>
      </el-col>

      <el-col :span='17'>
        <div class="often-goods">
          <div class="title">在售食品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click='addOrderList(goods)'>
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods-type">
          <el-tabs type='card'>
             <el-tab-pane label='汉堡'>
              <ul class='cookList'>
                <li v-for='goods in type0Goods' @click='addOrderList(goods)'>
                <span class="foodImg"><img v-bind:src="goods.goodsImg" width="100%"></span>
                <span class="foodName">{{goods.goodsName}}</span>
                <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
             </el-tab-pane>
             <el-tab-pane label="饮料">
               <ul class='cookList'>
                <li v-for='goods in type1Goods' @click='addOrderList(goods)'>
                <span class="foodImg"><img v-bind:src="goods.goodsImg" width="100%"></span>
                <span class="foodName">{{goods.goodsName}}</span>
                <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
             </el-tab-pane>
             <el-tab-pane label='零食'>
               <ul class='cookList'>
                <li v-for='goods in type2Goods' @click='addOrderList(goods)'>
                <span class="foodImg"><img v-bind:src="goods.goodsImg" width="100%"></span>
                <span class="foodName">{{goods.goodsName}}</span>
                <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
             </el-tab-pane>
             <el-tab-pane label='特价'>
               <ul class='cookList'>
                <li v-for='goods in type3Goods' @click='addOrderList(goods)'>
                <span class="foodImg"><img v-bind:src="goods.goodsImg" width="100%"></span>
                <span class="foodName">{{goods.goodsName}}</span>
                <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
             </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
      
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'pos',
  data (){
    return {
      tableData: [],

      //在售商品列
      oftenGoods:[],

      //详情商品列
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],

      totalCount:0,
      totalMoney:0,

    }
  },
  //钩子函数，创建时就开始拉取数据
  created:function (){
    axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods")
    .then(response=>{
      //console.log(response);
      this.oftenGoods = response.data;
    })
    .catch(error=>{
      console.log(error);
      alert('网络错误，访问失败！');
    })

    axios.get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods")
    .then(response=>{
      //console.log(response);
      this.type0Goods = response.data[0];
      this.type1Goods = response.data[1];
      this.type2Goods = response.data[2];
      this.type3Goods = response.data[3];
    })
    .catch(error=>{
      console.log(error);
      alert('网络错误，访问失败！');
    })
  },

  mounted: function (){
    var wHeight = document.body.clientHeight;
    console.log(wHeight);
    document.getElementById('pos-order').style.height = wHeight + 'px';
  },

  methods:{
    addOrderList(goods){

      //判断商品是否存在与订单列表中
      let isHave = false;
      for(let i=0;i<this.tableData.length;i++){
        if(this.tableData[i].goodsId===goods.goodsId){
          isHave = true;
        }
      };
      //根据判断再写逻辑
      if(isHave){
        let arr = this.tableData.filter(o=>o.goodsId === goods.goodsId);
        arr[0].count++;
      }else{
        let newGoods = {goodsId:goods.goodsId,
          goodsName:goods.goodsName,
          price:goods.price,
          count:1};
        this.tableData.push(newGoods);
      }

      this.getAllMoney();
    },

    //删除单个商品
    delSingleGoods(goods){
      console.log(goods);
      this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
      this.getAllMoney();
    },

    //删除所有商品
    delAllGoods(){
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },

    //汇总数量和金额
    getAllMoney(){
      this.totalCount=0;
      this.totalMoney=0;
      if(this.tableData){
        this.tableData.forEach((element) => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + (element.price*element.count);   
        });
      }
    },

    checkout(){
      if(this.totalCount != 0){
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message:'结账成功,用餐愉快',
          type:'success'
        });
      }else{
        this.$message.error('没有需要结账的商品！');
      }
    }

  }
}
</script>


<style scoped>
.pos-order{
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}

.btn-diancan{
  margin-top: 10px;
}

.title{
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  font-weight: 700;
}

.often-goods-list ul li{
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
  border-radius: 6px;
  cursor: pointer;
}

.o-price{
  color: rgb(202, 25, 25);
}

.goods-type{
  content: "";
  display: block;
  clear: both;
}

.cookList li{
  list-style: none;
  width:24%;
  border:1px solid #E5E9F2;
  height: auot;
  overflow: hidden;
  background-color:#fff;
  padding: 2px;
  float:left;
  margin: 2px;
  cursor: pointer;
} 

.cookList li span{
  display: block;
  float:left;
}

.foodImg{
  width: 40%;
}

.foodName{
  font-size: 18px;
  padding-left: 10px;
  color:brown;
}

.foodPrice{
  font-size: 16px;
  padding-left: 10px;
  padding-top:10px;
}

.totalDiv{
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}











</style>

