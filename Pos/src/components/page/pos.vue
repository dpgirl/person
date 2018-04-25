<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="名称"></el-table-column>
              <el-table-column prop="count" label="数量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingle(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <span><small>数量：</small>{{ totalCount }}</span>
              <span><small>金额：</small>{{ totalMoney }}元</span>
            </div>
            <div class="order-food-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAll()">删除</el-button>
              <el-button type="success" @click="checkOrder()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单"></el-tab-pane>
          <el-tab-pane label="外卖"></el-tab-pane>
        </el-tabs>

      </el-col>
      <el-col :span='17'>
        <div class="host-foods">
          <h3>热销商品</h3>
          <div>
            <ul>
              <li v-for="item in hotFoods" @click="addOrderList(item)">
                <span>{{item.goodsName}}</span>
                <span class="o-price">￥{{item.price}}</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="foods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <ul class='cookList'>
                  <li v-for="item in hunger" @click="addOrderList(item)">
                      <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                      <span class="foodName">{{ item.goodsName }}</span>
                      <span class="foodPrice">￥{{ item.price }}</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class='cookList'>
                  <li v-for="item in snack" @click="addOrderList(item)">
                      <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                      <span class="foodName">{{ item.goodsName }}</span>
                      <span class="foodPrice">￥{{ item.price }}</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class='cookList'>
                  <li v-for="item in drink" @click="addOrderList(item)">
                      <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                      <span class="foodName">{{ item.goodsName }}</span>
                      <span class="foodPrice">￥{{ item.price }}</span>
                  </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class='cookList'>
                  <li v-for="item in meal" @click="addOrderList(item)">
                      <span class="foodImg"><img :src="item.goodsImg" width="100%"></span>
                      <span class="foodName">{{ item.goodsName }}</span>
                      <span class="foodPrice">￥{{ item.price }}</span>
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
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      tableData: [],
      hotFoods: [
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
      hunger: [
         {
            goodsId: 1,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            goodsName: "香辣鸡腿堡",
            price: 18
        },
        {
            goodsId: 2,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            goodsName: "田园鸡腿堡",
            price: 15
        },
        {
            goodsId: 3,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            goodsName: "和风汉堡",
            price: 15
        }
      ],
      snack: [
         {
            goodsId: 4,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            goodsName: "大包薯条",
            price: 18
        },
        {
            goodsId: 5,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            goodsName: "脆皮炸鸡腿",
            price: 20
        },
        {
            goodsId: 6,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            goodsName: "魔法鸡块",
            price: 20
        }
      ],
      drink: [
         {
            goodsId: 7,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
            goodsName: "可乐大杯",
            price: 10
        },
        {
            goodsId: 8,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
            goodsName: "雪顶咖啡",
            price: 18
        }
      ],
      meal: [
         {
            goodsId: 9,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
            goodsName: "儿童欢乐套餐",
            price: 25
        },
        {
            goodsId: 10,
            goodsImg: "http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
            goodsName: "快乐全家桶",
            price: 99
        }
      ],
      totalMoney: 0,
      totalCount: 0
    };
  },
  // created() {
  //   //热销商品
  //   axios
  //     .get("http://jspang.com/DemoApi/oftenGoods.php")
  //     .then(resp => {
  //       //成功
  //       //  console.log(resp)
  //       this.hotFoods = resp.data;
  //     })
  //     .catch(resp => {
  //       //失败
  //       alert(resp, "网络失败");
  //     });
  //   //分类商品
  //   axios
  //     .get("http://jspang.com/DemoApi/typeGoods.php")
  //     .then(resp => {
  //       //成功
  //       //  console.log(resp)

  //       this.hunger = resp.data[0];
  //       this.snack = resp.data[1];
  //       this.drink = resp.data[2];
  //       this.meal = resp.data[3];
  //     })
  //     .catch(resp => {
  //       //失败
  //       alert(resp, "网络失败");
  //     });
  // },
  mounted: function() {
    var orderHeigh = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeigh + "px";
  },
  methods: {
    //添加到购物车，增加
    addOrderList(item) {
      // console.log(136,item)
      this.totalMoney = 0;
      this.totalCount = 0;
      let isHave = false;
      //判断这个商品是否已经存在于购物车中
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == item.goodsId) {
          isHave = true; //存在
        }
      }
      if (isHave) {
        //存在就进行数量添加
        let arr = this.tableData.filter(o => o.goodsId == item.goodsId);
        // console.log('arr',arr);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: item.goodsId,
          goodsName: item.goodsName,
          price: item.price,
          count: 1
        };
        this.tableData.push(newGoods);
        // console.log('tab',this.tableData)
      }

      this.getAllMoney();
    },
    //汇总数量和金额
    getAllMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;
      if (this.tableData) {
        this.tableData.forEach(element => {
          // console.log('e',element);
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.count * element.price;
        });
      }
    },
    //删除单个商品
    delSingle(item) {
      this.tableData = this.tableData.filter(o => o.goodsId != item.goodsId);
      this.getAllMoney();
    },
    //删除全部商品
    delAll() {
      this.tableData = [];
      this.totalMoney = 0;
      this.totalCount = 0;
    },
    //结账
    checkOrder() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
        this.$message({
          message: '结账成功，欢迎下次光临',
          type: 'success'
        })
      }else {
        this.$message.error('不能空结，请选择商品')
      }
    }
  }
};
</script>
<style scoped>
.pos-order {
  background-color: #fff;
  border-right: 1px solid #81b4db;
}
.order-food-btn {
  margin-top: 10px;
}
.host-foods {
}
.host-foods h3 {
  font-weight: normal;
  background-color: #ffffff;
  text-align: left;
  padding: 10px;
}
.host-foods li {
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.foods-type {
  clear: both;
}
.foods-type {
  padding: 0 8px;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 16px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.total {
  margin: 6px 0;
  border-bottom: 1px solid #e5e9f2;
}
.total span:first-child {
  margin-right: 10px;
}
</style>

