<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" id="pos-list">
        <template>
          <el-tabs>
            <el-tab-pane label="点餐">
              <template>
                <el-table :data="tableDatas" border style="width: 100%">
                  <el-table-column prop="goodsName" label="商品名称" width="90">
                  </el-table-column>
                  <el-table-column prop="count" label="数量" width="50">
                  </el-table-column>
                  <el-table-column prop="price" label="金额" width="50">
                  </el-table-column>
                  <el-table-column label="操作" fixed="right">
                    <template slot-scope="scope">
                      <el-button type="text" size="small" @click="deleteSingle(scope.row)">删除</el-button>
                      <el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
                    </template>
                  </el-table-column>
                </el-table>
              </template>
              <div>
                <span>总金额：{{totalPrice}}</span>&nbsp;&nbsp;&nbsp;<span>总数量：{{totalCount}}</span>
              </div>
              <div class="btns">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="deletaAll">删除</el-button>
                <el-button type="success" @click="checkOut">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单"></el-tab-pane>
            <el-tab-pane label="外卖"></el-tab-pane>
          </el-tabs>
        </template>
      </el-col>
      <el-col :span="17">
        <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">        
                <ul>
                    <li v-for="(good, index) in oftenGoods" :key="index" @click="addOrderList(good)">
                        <span>{{good.goodsName}}</span>
                        <span class="o-price">￥{{good.price}}元</span>
                    </li>        
                </ul>
            </div>
        </div>
        <div class="goods-type"> 
          <el-tabs>
              <el-tab-pane label="汉堡">
                  <ul class='cookList'>
                      <li v-for="(good,index) in type0Goods" :key="index" @click="addOrderList(good)">
                          <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                          <span class="foodName">{{good.goodsName}}</span>
                          <span class="foodPrice">￥{{good.price}}元</span>
                      </li>
                  </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                  <ul class='cookList'>
                      <li v-for="(good,index) in type1Goods" :key="index" @click="addOrderList(good)">
                          <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                          <span class="foodName">{{good.goodsName}}</span>
                          <span class="foodPrice">￥{{good.price}}元</span>
                      </li>
                  </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                  <ul class='cookList'>
                      <li v-for="(good,index) in type2Goods" :key="index" @click="addOrderList(good)">
                          <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                          <span class="foodName">{{good.goodsName}}</span>
                          <span class="foodPrice">￥{{good.price}}元</span>
                      </li>
                  </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                  <ul class='cookList'>
                      <li v-for="(good,index) in type3Goods" :key="index" @click="addOrderList(good)">
                          <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                          <span class="foodName">{{good.goodsName}}</span>
                          <span class="foodPrice">￥{{good.price}}元</span>
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
import axios from 'axios'
export default {
  name: "pos",
  data() {
    return {
      msg: "",
      tableDatas: [],
      oftenGoods:[],
      type0Goods:[],
      type1Goods:[],
      type2Goods:[],
      type3Goods:[],
      totalCount:0,
      totalPrice:0
    }
  },
  created(){
    axios.get('http://jspang.com/DemoApi/oftenGoods.php')
    .then(response=>{
      console.log(response)
      this.oftenGoods = response.data;
    })
    .catch(error=>{
      alert('请求接口错误')
    })

    axios.get('http://jspang.com/DemoApi/typeGoods.php')
    .then(response=>{
      this.type0Goods = response.data[0];
      this.type1Goods = response.data[1];
      this.type2Goods = response.data[2];
      this.type3Goods = response.data[3];
    })
    .catch(error=>{
      alert('请求接口错误')
    })
  },
  mounted() {
    var clientHeight = document.body.clientHeight;
    document.getElementById("pos-list").style.height = clientHeight + "px";
  },
  methods:{
    addOrderList(good){

      //判断列表里有没有点击的商品
      let isHas = false;
      this.tableDatas.forEach(item=>{
        if(item.goodsId==good.goodsId){
          isHas = true;
        } 
      });
      if(isHas){
        let currentGood = this.tableDatas.filter((item,index)=>{
          return item.goodsId == good.goodsId;
        });
        currentGood[0].count++;
      }else{
        let tableData = {goodsId:good.goodsId,price:good.price,goodsName:good.goodsName,count:1};
        this.tableDatas.push(tableData);
      } 
      this.getTotal();      
    },
    deleteSingle(good){
      let newDatas = this.tableDatas.filter((item,index) => item.goodsId != good.goodsId);
      this.tableDatas = newDatas;
      this.getTotal();
    },
    deletaAll(){
      this.tableDatas = [];
      this.totalCount = 0;
      this.totalPrice = 0;
    },
    getTotal(){
      this.totalCount = 0;
      this.totalPrice = 0;
      if(this.tableDatas){
          this.tableDatas.forEach(item=>{
          this.totalCount += item.count;
          this.totalPrice = this.totalPrice + item.price*item.count;
        });
      }
    },
    checkOut(){
      if(this.tableDatas.length!=0){
        this.tableDatas = [];
        this.totalCount = 0;
        this.totalPrice = 0;
        this.$message({
          message:'已经结账',
          type:'success'
        })
      }else{
        this.$message({
          message:'您没买商品',
          type:'error'
        })
      }   
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#pos-list {
  background: #f9fafc;
  border-right: 1px #c0ccda solid;
}
.btns{
  margin-top:20px;
}
.title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
}
.often-goods{
  text-align: left;
}
.often-goods-list{
  padding-left:10px;
}
.often-goods-list ul li{
  list-style: none;
  float:left;
  border:1px solid #E5E9F2;
  padding:10px;
  margin:5px;
  background-color:#fff;
}
.o-price{
  color:#58B7FF; 
}
.cookList li{
    list-style: none;
    width:23%;
    border:1px solid #E5E9F2;
    height: auot;
    overflow: hidden;
    background-color:#fff;
    padding: 2px;
    float:left;
    margin: 2px; 
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
.goods-type{
  clear:both;
  padding-left:15px;
}
.often-goods-list li{
  cursor:pointer;
}
</style>
