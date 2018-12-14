<template>
     <div class="pos">
         <el-row>
             <el-col :span="7" class="pos-order" id="order-list">
                                    
                    <el-tabs  type="card">
                        <el-tab-pane label="点餐">
                            <el-table width="100%" border :data="tableData">
                                <el-table-column prop="goodsName" label="商品名称" align="center" ></el-table-column>
                                <el-table-column prop="count" label="数量" width="80"></el-table-column>
                                <el-table-column prop="price" label="金额" width="80"></el-table-column>
                                <el-table-column label="操作" width="100" align="center" fixed="right">
                                    <template slot-scope="aa">
                                        <el-button type="text" size="small" @click="addOrderList(aa.row)">增加</el-button>
                                        <el-button type="text" size="small" @click="deleteSingle(aa.row)">删除</el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                            <div class="totalDiv">
                                数量 ： {{totalCount}} 金额 ：{{totalMoney}}元
                            </div>
                            <div class="div-btn">
                                <el-button type="warning">挂单</el-button>
                                <el-button type="danger" @click="deleteAll">删除</el-button>
                                <el-button type="success" @click="checkout">结账</el-button>
                            </div>
                        </el-tab-pane>
                        
                        <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
                        <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
                       
                    </el-tabs>
                    
             </el-col>
             <el-col :span="17">
                 <div class="often-list">
                     <div class="title">常用商品</div>
                     <div class="often-goods-list">
                         <ul>
                             <li v-for="goods in oftenGoods" :key="goods.goodsName" @click="addOrderList(goods)">
                                 <span>{{goods.goodsName}} </span>
                                 <span>{{goods.price}} </span>
                             </li>
                         </ul>
                     </div>
                 </div>
                 <div class="goods-type">
                     <el-tabs type="card">
                        <el-tab-pane label="汉堡">
                            <ul class="cookList">
                                <li v-for="typeGoods in type0Goods" :key="typeGoods.goodsName" @click="addOrderList(typeGoods)">
                                    <span class="foodImg"><img width="100%" :src="typeGoods.goodsImg"></span> 
                                    <span class="foodName">{{typeGoods.goodsName}}</span> 
                                    <span class="foodPrice">￥{{typeGoods.price}}</span> 
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="饮品">
                            <ul class="cookList">
                                <li v-for="typeGoods in type1Goods" :key="typeGoods.goodsName" @click="addOrderList(typeGoods)">
                                    <span class="foodImg"><img width="100%" :src="typeGoods.goodsImg"></span> 
                                    <span class="foodName">{{typeGoods.goodsName}}</span> 
                                    <span class="foodPrice">￥{{typeGoods.price}}</span> 
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="零食">
                            <ul class="cookList">
                                <li v-for="typeGoods in type2Goods" :key="typeGoods.goodsName" @click="addOrderList(typeGoods)">
                                    <span class="foodImg"><img width="100%" :src="typeGoods.goodsImg"></span> 
                                    <span class="foodName">{{typeGoods.goodsName}}</span> 
                                    <span class="foodPrice">￥{{typeGoods.price}}</span> 
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                            <ul class="cookList">
                                <li v-for="typeGoods in type3Goods" :key="typeGoods.goodsName" @click="addOrderList(typeGoods)">
                                    <span class="foodImg"><img width="100%" :src="typeGoods.goodsImg"></span> 
                                    <span class="foodName">{{typeGoods.goodsName}}</span> 
                                    <span class="foodPrice">￥{{typeGoods.price}}</span> 
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
    name:"pos",
    data(){
        return {
            tableData:[],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalMoney:0,
            totalCount:0
        }
    },
    created:function(){
        axios
            .get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods")
            .then(reponse=>{
                this.oftenGoods=reponse.data;
            })
            .catch(error=>{
                alert('网络错误');
            });
            axios
            .get("https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods")
            .then(reponse=>{
                this.type0Goods=reponse.data[0];
                this.type1Goods=reponse.data[1];
                this.type2Goods=reponse.data[2];
                this.type3Goods=reponse.data[3];
            })
            .catch(error=>{
                alert('网络错误');
            });
    },
    mounted:function () {
        var orderHeight=document.body.clientHeight;
        document.getElementById("order-list").style.height=orderHeight+'px';
    },
    updated:function () {

    },
    methods:{
        addOrderList:function(goods){
            this.totalCount=0;
            this.totalMoney=0;

            //判断商品是否已经存在订单列表
            let isHave=false;
            for(let i=0;i<this.tableData.length;i++){
                if(this.tableData[i].goodsId==goods.goodsId){
                    isHave=true;
                }
            }
            if(isHave){
                let arr=this.tableData.filter(o=>o.goodsId==goods.goodsId);
                arr[0].count++;
            }else{
                let newGoods={
                    goodsId:goods.goodsId,
                    goodsName:goods.goodsName,
                    price:goods.price,
                    count:1
                };
                this.tableData.push(newGoods);
            }
            this.querryAllGoods();
        },

        querryAllGoods:function(){
            this.totalCount=0;
            this.totalMoney=0;
            if(this.tableData){
                this.tableData.forEach(element => {
                    this.totalCount+=element.count;
                    this.totalMoney+=element.price*element.count;
                   // console.log(this.totalCount);    
                });
            }
        },
        deleteSingle:function(goods){
            this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId); //选出被选中的那个商品 把剩下的存入数组
            this.querryAllGoods();
        },
        deleteAll:function () {
            this.tableData=[];
            this.totalCount=0;
            this.totalMoney=0;

        },
        checkout:function () {
            if(this.tableData!=0){
                this.totalCount=0;
                this.totalMoney=0;
                this.tableData=[];
                this.$message({
                    message:'结账成功',
                    type:'success'
                })
            }else{
                this.$message.error('选点吃的在结账吧');
            }
        }

    }

};
</script>

<style>
.pos-order{
    background-color: gray;
    border: 1px solid gold;
}
.div-btn{
    margin-top: 10px;
}
.title{
    background-color: aliceblue;
    height: 20px;
    padding: 10px;
    text-align: left;
    border: 1px solid gray;
}
.totalDiv{
    padding: 10px;
    border: 1px solid gray;
}
.often-goods-list ul li{
    float: left;
    margin: 10px;
    padding: 8px;
    background-color: aliceblue;
}
.goods-type{
    clear: both;
    margin-top: 100px;
}
.cookList li{
    width: 25%;
    height: auto;
    overflow: hidden;
    padding: 2px;
    margin: 2px;
    border: 1px solid red;
    float: left;
}
.cookList li span{
    float: left;
    display: block;
}
.foodImg{
    width: 70px;
    margin: 3px;
    padding: 1px;
}
.foodPrice{
   margin-left: 15px;
   margin-top: 8px;
}
.foodName{
    margin: 5px 0px 0px 7px;
}
</style>
