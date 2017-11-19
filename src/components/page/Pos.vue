<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" class="pos-order" style="text-align:center" id="order-list">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table  :data="tableData" stripe border style="width:100%;">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="total">
                <span class="total-list">数量：</span>{{totalCount}} 个
                <span class="total-list">总价：</span>{{totalMoney}} 元
              </div>
              <div class="btn-list">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAllGoods">删除</el-button>
                <el-button type="success" @click="settleAccounts">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span="17">
          <div class="convenience-goods">
            <div class="title">常用商品</div>
            <div class="convenience-goods-list">
              <ul>
                <li v-for="goods in convenienceGoods" @click="addOrderList(goods)">
                  <span>{{goods.goodsName}}</span>
                  <span class="o-price">￥{{goods.price}}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList'>
                  <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class="cookList">
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class="cookList">
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                    <span class="foodName">{{goods.goodsName}}</span>
                    <span class="foodPrice">￥{{goods.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class="cookList">
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
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
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: 'Pos',
    data(){
        return {
          tableData: [],
          convenienceGoods:[
            {
              goodsId:1,
              goodsName:'香辣鸡腿堡',
              price:18
            }, {
              goodsId:2,
              goodsName:'田园鸡腿堡',
              price:15
            }, {
              goodsId:3,
              goodsName:'和风汉堡',
              price:15
            }, {
              goodsId:4,
              goodsName:'快乐全家桶',
              price:80
            }, {
              goodsId:5,
              goodsName:'脆皮炸鸡腿',
              price:10
            }, {
              goodsId:6,
              goodsName:'魔法鸡块',
              price:20
            }, {
              goodsId:7,
              goodsName:'可乐大杯',
              price:10
            }, {
              goodsId:8,
              goodsName:'雪顶咖啡',
              price:18
            }, {
              goodsId:9,
              goodsName:'大块鸡米花',
              price:15
            }, {
              goodsId:20,
              goodsName:'香脆鸡柳',
              price:17
            }
          ],
          type0Goods:[
            {
              goodsId:1,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
              goodsName:'香辣鸡腿堡',
              price:18
            }, {
              goodsId:2,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
              goodsName:'田园鸡腿堡',
              price:15
            }, {
              goodsId:3,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
              goodsName:'和风汉堡',
              price:15
            }, {
              goodsId:4,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
              goodsName:'快乐全家桶',
              price:80
            }, {
              goodsId:5,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
              goodsName:'脆皮炸鸡腿',
              price:10
            }, {
              goodsId:6,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
              goodsName:'魔法鸡块',
              price:20
            }, {
              goodsId:7,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
              goodsName:'可乐大杯',
              price:10
            }, {
              goodsId:8,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
              goodsName:'雪顶咖啡',
              price:18
            }, {
              goodsId:9,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
              goodsName:'大块鸡米花',
              price:15
            }, {
              goodsId: 20,
              goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
              goodsName: '香脆鸡柳',
              price: 17
            }
          ],
          type1Goods:[],
          type2Goods:[],
          type3Goods:[],
          totalMoney: 0,
          totalCount: 0
        }
    },
    created:function(){
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
          .then(reponse=>{
          })
          .catch(error=>{
              console.log(error);
          });
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
          .then(reponse=>{
            this.type0Goods = reponse.data[0];
            this.type1Goods = reponse.data[1];
            this.type2Goods = reponse.data[2];
            this.type3Goods = reponse.data[3];
          })
          .catch(error=>{
              console.log(error);
              alert('网络错误，不能访问');
          })
    },
    mounted: function(){
        var orderHeight = document.body.clientHeight;
        document.getElementById('order-list').style.height = orderHeight+'px';
    },
    methods:{
      addOrderList(goods){
          this.totalCount = 0;
          this.totalMoney = 0;

          let isHave = false;
          let isFalse = isHave;
          //商品是否已经存在于订单列表中
          for(let i=0; i<this.tableData.length; i++){
            if(this.tableData[i].goodsId===goods.goodsId){
                isHave=true;
            }
          }
            //根据判断的值编写业务逻辑
        if(isHave){
            let arr = this.tableData.filter(o => o.goodsId === goods.goodsId);
            arr[0].count++;
        }else {
          let newGoods = {
            goodsId: goods.goodsId,
            goodsName: goods.goodsName,
            price: goods.price,
            count: 1
          };
          this.tableData.push(newGoods);
        };
        this.getAllMoney();
      },
      delSingleGoods(goods){
        this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
        this.totalCount -= goods.count;
        this.totalMoney -= goods.price*goods.count;
      },
      getAllMoney(){
        this.tableData.forEach((element) => {
          this.totalCount+=element.count;
          this.totalMoney = this.totalMoney+(element.price*element.count);
        })
      },
      delAllGoods(){
          this.tableData=[];
          this.totalCount = 0;
          this.totalMoney = 0;
      },
      settleAccounts(){
          if(this.totalCount){
            this.$message({
              message:'商品数量为：'+this.totalCount+'   商品总金额为：'+this.totalMoney+'元。 恭喜结账成功',
              type:'success'
            });
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;

          }else{
            this.$message.error('不能空结');
          }
      }
    }
  }

</script>

<style scoped>
  .goods-type{
    clear:both;
  }
  .pos-order{
    background-color: #F9FAFC;
    border-right:1px solid #C0CCDA;
  }
  .btn-list {
    margin:10px;
  }
  .title{
    height: 20px;
    border-bottom: 1px solid #D3dce6;
    background-color: #F9FAFC;
    padding: 10px;
    text-align: left;
  }
  .convenience-goods-list ul li {
    list-style: none;
    float: left;
    borrder: 1px solid #E5E9F2;
    padding: 10px;
    margin: 10px;
    background-color: #FFF;
  }
  .o-price{
    color: #58B7FF
  }
  .cookList li{
    list-style: none;
    width:23%;
    border:1px solid #E5E9F2;
    height: auto;
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
  .total{
    height: 25px;
    padding: 10px;
    margin-top: 10px;

    background-color: #8cc5ff;
    border-right:1px solid #C0CCDA;
  }
</style>

