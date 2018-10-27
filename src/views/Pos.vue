<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span='payList' class='pos-order' id='order-list'>
            <el-tabs v-model="activeName" type="border-card" @tab-click="handleClick" >
              <el-tab-pane label="结账" name="first">
                <el-table
                  :data="tableData"
                  border
                  stripe
                  size='middle'
                  class='ol-tableHeight'
                  height='ol-tableHeight'
                  >
                  <el-table-column
                    prop="goodsName"
                    label="商品名称"
                    show-overflow-tooltip
                    align='center'
                    >
                  </el-table-column>
                  <el-table-column
                    prop="price"
                    label="金额"
                    width='80'
                    align='center'
                    >
                  </el-table-column>
                  <el-table-column
                    prop="count"
                    label="数量"
                    width='80' 
                    align='center'

                    >
                  </el-table-column>
                  <el-table-column
                    prop="options"
                    label="操作"
                    width='80'  
                    align='center'

                    >
                      <template slot-scope="scope">
                        <div class="btns">
                          <el-button @click="add(scope.row)" type="text" size="medium">增加</el-button>
                          <el-button @click="reduce(scope.row)" type="text" size="medium">减少</el-button>
                        </div>
                        
                      </template>
                  </el-table-column>
                </el-table>
                <div class="checkOut">
                  <span>数量：<b>{{getCount}}</b></span>
                  <span>总价：<b>￥{{getAllPrice}}</b></span>
                </div>
                <div class="btns">
                  <el-button type="success" @click='checkOut'>结账</el-button>
                  <el-button type="primary">挂单</el-button>
                  <el-button type="danger" @click='deleteList'>删除</el-button>
                </div>
              </el-tab-pane>
              <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
              <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
            </el-tabs>
            <!-- 弹出支付对话框 -->
            <el-dialog title="结账（单位：元）" :visible.sync="dialogFormVisible">
                  <el-form :model="form" :rules="rules" ref="form">
                    <el-form-item label="应付金额" :label-width="formLabelWidth" >
                      <el-input v-model="allPrice" autocomplete="off" :disabled="true"></el-input>
                    </el-form-item>
                    <el-form-item label="实际支付" :label-width="formLabelWidth" prop="pay">
                      <el-input v-model="form.pay" autocomplete="off" autofocus='true'></el-input>                    
                    </el-form-item>
                    <el-form-item label="找零" :label-width="formLabelWidth">
                      <el-input v-model="getChange" autocomplete="off" :disabled="true"></el-input>                    
                    </el-form-item>
                  </el-form>
                  <div slot="footer" class="dialog-footer">
                    <el-button @click="dialogFormVisible = false">取 消</el-button>
                    <el-button type="primary" @click="confirmCheck">确 定</el-button>
                  </div>
                </el-dialog>
        </el-col>
        <el-col :span='goodsList'>
          <div class="hotSale">
            <div class="title">热卖商品</div>
            <div class="hotList" v-loading="loading1">
              <ul>
                <li v-for='(item,index) in hotGoods' :key='index'  @click='addOrderList(item)'>
                  <span>{{item.goodsName}}</span>
                  <span class='o-price'>￥{{item.price}}</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goodType">
            <el-tabs type="border-card" v-loading="loading2">
              <el-tab-pane label="汉堡">
                <ul class="cookList">
                  <li v-for='(item,index) in goodsType1'
                  :key='index'
                  @click='addOrderList(item)'
                  >
                    <img class='foodImg' src='http://p0.meituan.net/210.0/wmproduct/29ab89aeb0d7c19d649dbc55e70e56fd170289.jpg' alt="">
                    <div>
                      <span class='foodName'>{{item.goodsName}}</span>
                      <span class='foodPrice'>￥{{item.price}}</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class="cookList">
                  <li v-for='(item,index) in goodsType2'
                :key='index'
                @click='addOrderList(item)'>
                    <img class='foodImg' src='http://p0.meituan.net/210.0/wmproduct/9e1e7bed06c213742038c556087cee3c270995.jpg' alt="">
                    <div>
                      <span class='foodName'>{{item.goodsName}}</span>
                      <span class='foodPrice'>￥{{item.price}}</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class="cookList">
                  <li v-for='(item,index) in goodsType3'
                :key='index'
                @click='addOrderList(item)'>
                    <img class='foodImg' src='http://p0.meituan.net/210.0/wmproduct/ed5409c9bbb9305e9403393751d993d168667.jpg' alt="">
                    <div>
                      <span class='foodName'>{{item.goodsName}}</span>
                      <span class='foodPrice'>￥{{item.price}}</span>
                    </div>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class="cookList">
                  <li v-for='(item,index) in goodsType4'
                :key='index'
                @click='addOrderList(item)'>
                    <img class='foodImg' src='http://p1.meituan.net/210.0/wmproduct/92b0ac26382f1b42f6bdbb6caa4ed6a8217452.jpg' alt="">
                    <div>
                      <span class='foodName'>{{item.goodsName}}</span>
                      <span class='foodPrice'>￥{{item.price}}</span>
                    </div>
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
/* eslint-disable */
import axios from 'axios'
export default {
  name: "Pos",
  data() {
    // 自定义规则失效--比较两个input值之间的大小
    var compare = function(rule,value,callback){
      if(!value){
        return callback(new Error('支付金额不能为空！'))
      }
      setTimeout(() => {
        if(!Number(value)){
          callback(new Error('请输入数字'));
          } else {
          // 这里的this.allPrice 无法读取
          if (value < this.allPrice){
            callback(new Error('支付金额不足'));
          } 
          else {
            callback();
          }
        }
      },1000);
    };
    return {
      payList: 6,
      goodsList:18,
      activeName: 'first',
      tableData: [],
      tableHeight:'',
      hotGoods:[],
      goodsType1:[],
      goodsType2:[],
      goodsType3:[],
      goodsType4:[],
      dialogFormVisible:false,
      allPrice: '',
      change:'',
      formLabelWidth: '120px',
      form: {
          pay: '',
          delivery: false,
          type: [],
          resource: '',
          desc: ''
      },
      rules: {
        pay:[
          {validator: compare, trigger: 'blur'}
        ]
      },
      loading1: true,
      loading2: true,

    }
  },
  computed: {
    getAllPrice(){
      let sum = 0;
      for(var i = 0,len=this.tableData.length;i<len;i++){
        sum += this.tableData[i].count * this.tableData[i].price
      }
      this.allPrice = sum
      return sum;
    },
    getCount(){
      let sum = 0;
      for(var i = 0,len=this.tableData.length;i<len;i++){
        sum += this.tableData[i].count;
      }
      return sum;
    },
    getChange(){
      if (this.form.pay && this.form.pay >= this.allPrice) {
        this.change = this.form.pay - this.allPrice;
      }else {
        if (this.form.pay && this.form.pay < this.allPrice){
          this.change = '支付金额不足！'
        } else {
          this.change = '';
        }
      }
      return  this.change 
    },
    // myFocus(){
    //   this.$refs.focus.focus();
    // }
  },
  created(){
    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods').then(res => {
      // console.log(res)
      if (res.status === 200){
        this.hotGoods = res.data;
        this.loading1 = false;
      }
    }),
    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods').then(res => {
      // console.log(res)
      if (res.status === 200) {
        this.goodsType1 = res.data[0];
        this.goodsType2 = res.data[1];
        this.goodsType3 = res.data[2];
        this.goodsType4 = res.data[3];
        this.loading2 = false
      }
    })

    
  },
  mounted() {
    var orderHeight = document.body.clientHeight;
    // console.log(orderHeight)
    document.getElementById("order-list").style.height = orderHeight + "px";
    var screenWidth = document.body.clientWidth;
    if (screenWidth < 760) {
      this.payList = 12;
      this.goodsList = 12;
    }
    var elTable = document.getElementsByClassName('el-tabs__content')[0];
    var elNav = document.getElementsByClassName('el-tabs__nav-scroll')[0];
    var tableHeight = document.getElementsByClassName('ol-tableHeight')[0];
    var checkOut = document.getElementsByClassName('checkOut')[0];
    var btns = document.getElementsByClassName('btns')[0];
    // elTable.style.height = orderHeight - elNav.clientHeight + 'px';
    elTable.style.height = orderHeight - 40 + 'px';
    tableHeight.style.height = orderHeight - 40 - 30 - checkOut.clientHeight - btns.clientHeight +'px'
    // console.log(tableHeight.style.height)
    // wtf? 还没加载出来，elNav.clientHeight 的值为0
    // console.log(elNav,elNav.style.height)
    // console.log(btns.clientHeight)
  },
  updated() {
       // 设置快餐分类区高度
    var orderHeight = document.body.clientHeight;
    var hotsale = document.getElementsByClassName('hotSale')[0];
    var elTable2 = document.getElementsByClassName('el-tabs__content')[1];
    var elNav = document.getElementsByClassName('el-tabs__nav-scroll')[0]
    console.log(elTable2)
    console.log(hotsale.clientHeight) // 41 + hotList.clientWidth 
    elTable2.style.height = orderHeight - hotsale.clientHeight - elNav.clientHeight - 30 +'px';
  },
 
  methods: {
    handleClick(tab,event) {
      // console.log(tab, event);
    },
    add(row){
      // console.log(row)
      row.count++ 
    },
    reduce(row){
      // console.log(row)
      row.count--
      if (row.count === 0){
        for (var i=0,len=this.tableData.length;i<len;i++){
          if (row.goodsId === this.tableData[i].goodsId) {
            this.tableData.splice(i,1)
          }
      }
      }
    },
    addOrderList(item){
      // console.log(item)
      let isHas = false;
        // 判断商品是否存在
      for (var i=0,len=this.tableData.length; i<len; i++) {
        if(this.tableData[i].goodsId == item.goodsId) {
          isHas = true;
        } 
      }
      // 根据判断结果添加数据
      if (isHas) {
        // 过滤出id等于传进来的数据的id
        let arr = this.tableData.filter(index => index.goodsId == item.goodsId)
        arr[0].count++;
      } else {
        let newTableData = {
          goodsId: item.goodsId, 
          goodsName: item.goodsName, 
          price: item.price, 
          count: 1
        }
        this.tableData.push(newTableData)
      }
    },
    deleteList(){
      this.$confirm('确定删除该订单, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.tableData = [];
          this.$message({
            type: 'warning ',
            message: '已删除!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
    },
    checkOut(){
      this.dialogFormVisible = true;
    },
    confirmCheck(){
      if (this.form.pay!=='' && this.change!=='支付金额不足！'){
        this.dialogFormVisible = false;
        this.$message({
          message: '结账成功',
          type: 'success',
          duration: '1000'
        })
        this.tableData = [];
        this.form.pay = '';
      }
    }
  }
};
</script>

<style lang='less' scoped>
.pos-order{
  background-color: #f9fafc;
  border-right: 1px solid #c0cdcc;
  overflow: hidden;
  .ol-tableHeight {
    height: 200px;
  }
  .checkOut {
    padding: 1.25rem;
    display: flex;
    // width: 100%;
    justify-content: space-around;
    border-bottom: 1px solid #c0cdcc;
    margin-bottom: 1.25rem;
  }
  .btns {
    display: flex;
    justify-content: center;
    align-content: center;
  }
}
.hotSale {
  // overflow: auto;
  .title {
    height: 2.5rem;
    line-height:  2.5rem;
    padding-left: 0.62rem;
    border-bottom: 1px solid #ccc;
    background-color: #f9fafc;
  }
  .hotList ul {
    display: flex;
    width: 100%;
    justify-content: center;
    flex-wrap: wrap;
    li{
      width: 20%;
      border: 1px solid #e5e9f2;
      padding: 0.62rem;
      height: 2rem;
      line-height: 2rem;
      margin: 1.25rem 0.31rem;
      text-align: center;
      background-color: #fff;
      cursor: pointer;
      .o-price {
        color: orange;
      }
    }
    @media screen and (max-width: 768px){
      li{
        width: 15%;
        overflow: hidden;
        font-size: 0.8rem;
        border: 1px solid #e5e9f2;
        padding: 0.3rem;
        line-height: 2rem;
        margin: 0;
      }
    }
  }
  
  
}
.goodType {
  .cookList {
    display: flex;
    width: 100%;
    flex-wrap: wrap;
    justify-content: center;
    li {
      width: 15%;
      min-width: 140px;
      border: 1px solid #ccc;
      padding: 0.3rem;
      margin: 1rem;
      display: flex;
      cursor: pointer;
      .foodImg {
          width: 60%;
          height: 100%;
        }
      div {
        display: flex;
        font-size: 1rem;
        width: 100%;
        text-align: center;
        flex-wrap: wrap;
        justify-content: center;
        align-content: center;
        .foodName {
          color: brown;
        }
        .foodPrice {
          display: block;
          margin-top: 0.62rem;
        }
      }
    }
    
  }
  @media screen and (max-width: 768px){
    .cookList{
      display: flex;
      overflow: scroll;
      flex-wrap: nowrap;
      justify-content: flex-start;
      li {
        margin: 0.2rem;
        span {
          font-size: 0.9rem;
        }
      }
    }
        
    }
  
  
}
</style>
