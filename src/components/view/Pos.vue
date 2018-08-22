<template>
    <div class="pos">
        <el-row>
            <el-col :span="7"
                    class="Collecting"
                    id="Collecting">
                <el-tabs border
                         show-summary
                         style="text-align:center">
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData"
                                  border
                                  style="width: 100%;">
                            <el-table-column prop="goodsName"
                                             label="商品名称"> </el-table-column>
                            <el-table-column prop="count"
                                             label="数量"> </el-table-column>
                            <el-table-column prop="price"
                                             label="金额"> </el-table-column>
                            <el-table-column fixed="right"
                                             label="操作"
                                             width="100">
                                <template slot-scope="scope">
                                    <el-button type="text"
                                               size="small"
                                               @click="removeList(scope.$index)">删除</el-button>
                                    <el-button type="text"
                                               size="small"
                                               @click="addOuderList(scope.row)">添加</el-button>
                                    <!-- 这里是饿了么规定的 scope.row 就是这一整行数据 -->
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">挂单</el-tab-pane>
                    <el-tab-pane label="外卖">外卖</el-tab-pane>
                </el-tabs>
                <div class="pos-info">
                    <span>
                        <small>数量：</small>{{ listNumber }}</span>
                    <span>
                        <small>金额：</small>{{ listMoney }}</span>
                </div>
                <div class="pos-scope">
                    <el-button type="primary">挂单</el-button>
                    <el-button type="danger"
                               @click="removeAll">删除</el-button>
                    <el-button type="success"
                               @click="clickMoney">结账</el-button>
                </div>

            </el-col>
            <el-col :span="15">
                <div class="commodity">
                    <div class="top-commodity">
                        <div class="title">热销商品</div>
                        <div class="commodity-list">
                            <ul>
                                <li v-for="goods in oftenGoods"
                                    @click="addOuderList(goods)"
                                    :key="goods.goodsId">
                                    <span>{{ goods.goodsName }}</span>
                                    <span>￥{{ goods.price }}元</span>
                                </li>
                            </ul>
                        </div>
                    </div>
                    <div class="commodity-fication">
                        <el-tabs>
                            <el-tab-pane label="汉堡">
                                <ul class='cookList'>
                                    <li v-for="goods in type0Goods"
                                        @click="addOuderList(goods)"
                                        :key="goods.goodsId">
                                        <span class="foodImg"><img :src="goods.goodsImg"
                                                 width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="小食">
                                <ul class='cookList'>
                                    <li v-for="goods in type1Goods"
                                        @click="addOuderList(goods)"
                                        :key="goods.goodsId">
                                        <span class="foodImg"><img :src="goods.goodsImg"
                                                 width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <ul class='cookList'>
                                    <li v-for="goods in type2Goods"
                                        @click="addOuderList(goods)"
                                        :key="goods.goodsId">
                                        <span class="foodImg"><img :src="goods.goodsImg"
                                                 width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="套餐">
                                <ul class='cookList'>
                                    <li v-for="goods in type3Goods"
                                        @click="addOuderList(goods)"
                                        :key="goods.goodsId">
                                        <span class="foodImg"><img :src="goods.goodsImg"
                                                 width="100%"></span>
                                        <span class="foodName">{{ goods.goodsName }}</span>
                                        <span class="foodPrice">￥{{ goods.price }}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>

                        </el-tabs>
                    </div>
                </div>
            </el-col>
        </el-row>
    </div>
</template>
    
<script>
import axios from 'axios'
    export default {
        name: 'pos',
        data(){
            return{
                tableData:[],
                oftenGoods:[],
                type0Goods:[],
                type1Goods:[],
                type2Goods:[],
                type3Goods:[],
                listNumber:0,
                listMoney:0
            }
        },
        created(){
            axios.get('http://jspang.com/DemoApi/oftenGoods.php')
                .then(data=>{
                    this.oftenGoods = data.data;
                })
                .catch(error=>{
                    alert('网络连接错误')
                })
            axios.get('http://jspang.com/DemoApi/typeGoods.php')
                .then(data=>{
                   this.type0Goods = data.data[0];
                   this.type1Goods = data.data[1];
                   this.type2Goods = data.data[2];
                   this.type3Goods = data.data[3];
                    
                })
                .catch(error=>{
                    alert('网络连接错误')
                })
        },
        mounted(){
            let windowHeight = document.body.clientHeight;
            document.getElementById('Collecting').style.height=windowHeight+'px';
        },
        methods:{
            addOuderList(data){
                // 首先 判断 购物车里面是否存在这个商品。
                let isHave = false;
                let isId = null;
                for(let i = 0;i < this.tableData.length;i++){
                    if(this.tableData[i].goodsId == data.goodsId){ //判断购物车里面是否存在新商品的id即可
                        isHave = true;
                        isId = i;
                    }
                }
                // 如果存在 则添加数量   如果不存在 则添加整条数据
                if(isHave){
                    // 存在，改变数量
                    this.tableData[isId].count++;
                }else{
                    let newArr = {goodsId:data.goodsId,goodsName:data.goodsName,price:data.price,count:1}
                    this.tableData.push(newArr)
                }
                this.getAllMoney()
            },
            removeList(id){
                // 删除购物车单个商品
                if(this.tableData[id].count > 1){
                    this.tableData[id].count--;
                }else{
                    this.tableData.splice(id,id+1);
                }
                this.getAllMoney()
            },
            getAllMoney(){
                // 金额计算器
                this.listNumber = this.listMoney =0
                if(this.tableData){
                    this.tableData.forEach((thisDta)=>{
                        this.listNumber += thisDta.count;
                        this.listMoney = this.listMoney + (thisDta.price * thisDta.count);
                    })
                }
            },
            clickMoney(){
                if(this.listNumber == 0){
                    this.$message.error('不能空结！');
                }else{
                    this.tableData = []
                    this.listNumber = 0
                    this.listMoney = 0 
                    this.$message.success('结账成功');
                }
            },
            removeAll(){
                // 删除购物车所有商品
                this.tableData = []
                this.listNumber = 0
                this.listMoney = 0 
            }
        }
    }
</script>

<style lang="stylus">
.Collecting {
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
}

.pos-scope {
    margin-top: 20px;
}

.pos-info {
    margin-top: 20px;
    background: #ffffff;
    border-bottom: 1px solid #C0ffDA;
    padding-bottom: 20px;
    font-size: 20px;
}

.pos-info span {
    margin: 0 20px;
}

.title {
    padding: 10px;
    font-size: 14px;
    text-align: left;
    border-bottom: 1px solid #C0CCDA;
}

.el-tabs--bottom .el-tabs__item.is-bottom:nth-child(2), .el-tabs--bottom .el-tabs__item.is-top:nth-child(2), .el-tabs--top .el-tabs__item.is-bottom:nth-child(2), .el-tabs--top .el-tabs__item.is-top:nth-child(2) {
    padding-left: 20px;
}

.commodity-list ul {
    padding: 10px;
}

.commodity-list ul li {
    list-style: none;
    border: 1px solid #C0CCDA;
    display: inline-block;
    padding: 10px 15px;
    margin: 8px 10px;
    text-align: left;
}

.commodity-fication {
    margin-top: 20px;
    padding: 10px;
}

.cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auot;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
}

.cookList li span {
    display: block;
    float: left;
}

.foodImg {
    width: 40%;
}

.foodName {
    font-size: 18px;
    padding-left: 10px;
    color: brown;
}

.foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
}
</style>
    