<template>
<el-container style="height: 100%; border: 1px solid #eee">

<el-container>
<!--    顶部栏目展示  -->
    <el-header>
        <el-row :gutter="20">
            <el-col class='col1' :class="{activeClass: orderValue}" ><div @click='showCreateOrder()'>创建订单</div></el-col>
            <el-col class='col1' :class='{activeClass: !orderValue}' ><div @click='showMyOrder()'>我的订单</div></el-col>
            <el-col class='col2'><el-link  type="primary" :underline="false"><i class="el-icon-headset"></i>联系客服</el-link></el-col>
            <el-col class='col2'><el-link  type="primary" :underline="false"><i class="el-icon-s-order"></i>新建订单</el-link></el-col>
        </el-row>
    </el-header>
<!--    创建订单-->
    <el-main v-show='orderValue'>
        <!-- 非标定制进度步骤 -->
        <el-steps :active="activeStep" finish-status="success" >
            <el-step title="需求填写"></el-step>
            <el-step title="待分析"></el-step>
            <el-step title="待报价"></el-step>
            <el-step title="待确认"></el-step>
            <el-step title="完成"></el-step>
        </el-steps>
        <br />

        <!-- 订单创建模块 -->
        <!-- 需求填写 -->
        <el-form v-show='orderCreate'>
            <el-row><img src='../../assets/img/littleBlue.png' style='vertical-align: middle'>&nbsp;需求填写</el-row><br />
            <!-- 机械性能选择 -->
            <div class='strength-choose'>
                <el-row style='font-weight: bold'>机械性能选择</el-row><br />
                <el-radio-group size='1' v-model="radio1" >
                    <span @click='strengthChoose1()'><el-radio-button label="1">螺栓</el-radio-button></span>
                    <span @click='strengthChoose1()'><el-radio-button label="2">螺柱</el-radio-button></span>
                    <span @click='strengthChoose1()'><el-radio-button label="3">螺钉</el-radio-button></span>
                    <span @click='strengthChoose2()'><el-radio-button label="4">螺母</el-radio-button></span>
                    <span @click='strengthChoose3()'><el-radio-button label="5">垫圈</el-radio-button></span>
                    <span @click='strengthChoose4()'><el-radio-button label="6">销轴</el-radio-button></span>
                    <span @click='strengthChoose5()'><el-radio-button label="7">其他</el-radio-button></span>
                </el-radio-group>
                <br /><br />
                <el-row style='width: 140px;font-size: 14px;margin-bottom: 5px'>
                    <el-col :span=12 >强度</el-col>
                    <el-col :span=12>自定义数值</el-col>
                </el-row>
<!--                螺栓/螺柱/螺钉的强度参数-->
                <el-row v-show='strengthNumberALL[0]'>
                    &nbsp;&nbsp;&nbsp;
                    <el-radio v-model="radio2" label="1">4.8</el-radio>
                    <el-radio v-model="radio2" label="2">8.8</el-radio>
                    <el-radio v-model="radio2" label="3">10.9</el-radio>
                    <el-radio v-model="radio2" label="4">12.9</el-radio>
                </el-row>
<!--                螺母的强度参数-->
                <el-row v-show='strengthNumberALL[1]'>
                    &nbsp;&nbsp;&nbsp;
                    <el-radio v-model="radio2" label="1">4</el-radio>
                    <el-radio v-model="radio2" label="2">5</el-radio>
                    <el-radio v-model="radio2" label="3">6</el-radio>
                    <el-radio v-model="radio2" label="4">8</el-radio>
                    <el-radio v-model="radio2" label="5">10</el-radio>
                    <el-radio v-model="radio2" label="6">12</el-radio>
                </el-row>
 <!--                垫圈的强度参数-->
                <el-row v-show='strengthNumberALL[2]'>
                    &nbsp;&nbsp;&nbsp;
                    <el-radio v-model="radio2" label="1">100HZ</el-radio>
                    <el-radio v-model="radio2" label="2">200HZ</el-radio>
                    <el-radio v-model="radio2" label="3">300HZ</el-radio>
                    <el-radio v-model="radio2" label="4">HRC35-45</el-radio>
                </el-row>
<!--                销轴的强度参数-->
                <el-row v-show='strengthNumberALL[3]'>
                    &nbsp;&nbsp;&nbsp;
                    请填写强度参数范围：<el-input style='width: 100px'></el-input>
                </el-row>
<!--                其他的强度参数-->
                <el-row v-show='strengthNumberALL[4]'>
                    <el-select v-model="materialValue" placeholder="请选择材质">
                        <el-option
                            v-for="item in options2"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value"
                            :disabled="item.disabled">
                        </el-option>
                    </el-select>
                    <br/><br/>
                    <span style='font-size: 14px'>
                        请填写其他参数要求：<el-input style='width: 120px'></el-input>（如抗拉强度，屈服轻度、硬度、伸长率、收缩率等）
                    </span>
                </el-row>
            </div>
            <br /><br />
            <!-- 表面处理选择 -->
            <div class='superficial-treatment'>
                <el-row  style='font-weight: bold'><img src='../../assets/img/littleBlue.png' style='vertical-align: middle'>表面处理</el-row>
                <br />
                <el-radio-group size='medium' v-model="radio3" style='margin-bottom: 5px'>
                    <el-radio-button label="1">防腐处理</el-radio-button>
                    <el-radio-button label="2">强化处理</el-radio-button>
                    <el-radio-button label="3">自定义</el-radio-button>
                </el-radio-group>
                <el-row>
                    <el-select v-model="ffValue" placeholder="请选择">
                        <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value"
                            :disabled="item.disabled">
                        </el-option>
                    </el-select>
                </el-row>
            </div>
            <br /><br />
            <!-- 数量选择 -->
            <div>
                <el-row style='font-weight: bold'>所需数量</el-row>
                <br>
                <el-row  style='font-size:14px;color: #F56C6C'>起购数量：9.87千支</el-row>
                <el-row>
                    <el-input-number v-model="num" controls-position="right" @change="handleChange" :min="1" :max="10"></el-input-number>箱
                    <el-input
                        placeholder="请输入内容"
                        v-model="thousands"
                        clearable
                        style='width: 100px'>
                    </el-input>千支&nbsp;&nbsp;
                    <div style='display:inline-block;font-size:14px;width:150px;height:30px;
                                  line-height:30px;text-align: center;
                                  background-color: rgb(255,243,234);'>
                        已选金额：待报价
                    </div>
                </el-row>
            </div>

            <br/>
            <el-button @click="dialogTableVisible = true" type='primary' style='width: 300px;height: 40px; margin-left: 50px'>确认提交</el-button>
            <el-dialog :visible.sync="dialogTableVisible" top='15%' modal=false>
                <el-row>
                    <img src='../../assets/img/success.png' />
                </el-row>
                <el-row style='font-weight: bold;font-size: 20px'>需求提交成功</el-row><br>
                <el-row>请您耐心等待，我们将尽快进行订单信息的确认</el-row><br>
                <el-button type='primary'>继续创建</el-button> <el-button @click='stepNext()'>返回页面</el-button>
            </el-dialog>
        </el-form>
<!--        订单分析-->
        <el-form v-show='orderExplain'>
            <div class='priceExplain-css' >
                <br>
                <div><img src='../../assets/img/waitExplain.jpg'></div>
                <div style='font-size:24px;font-weight: bold;color:rgb(111,39,19);margin-bottom: 20px;'>订单分析中...</div>
                <el-text style='font-size:14px;'>
                   请您耐心等待，我们将对订单信息进行确认
                </el-text>
            </div>
        </el-form>
<!--        价格待分析-->
        <el-form v-show='priceExplain'>
            <div class='priceExplain-css' >
                <br>
                <div><img src='../../assets/img/waitExplain.jpg'></div>
                <div style='font-size:24px;font-weight: bold;color:rgb(97,241,4);margin-bottom: 20px;'>等待报价中...</div>
                <el-text style='font-size:14px;'>
                    请您耐心等待，我们将对订单价格进行确认
                </el-text>
            </div>
        </el-form>
<!--        待确认-->
        <el-form v-show='orderConfirm'>
            <div class='orderList'>
                <el-row class='orderListTop'>
                    <el-col :span='23'>
                        <el-checkbox></el-checkbox>&nbsp;&nbsp;订单编号(非标订单)：
                        <span>3326522346465</span>
                    </el-col>
                    <el-col :span='1'>
                        <span @click='alert("确认删除？")'><i class="el-icon-delete" ></i></span>
                    </el-col>
                </el-row>
                <!--            订单信息-->
                <el-row class='orderListMain'>
                    <el-col :span='3' align='middle' style='margin-left: 10px'>
                        <div style='line-height: 80px'>
                            商品图片
                        </div>
                    </el-col>
                    <el-col :span='6'>
                        <el-row style='line-height: 35px'><span style='font-weight: bold'>U形螺栓JB/ZQ 1234- -2000...</span></el-row>
                        <el-row style='line-height: 35px'>物料编号<span>115646541</span></el-row>
                    </el-col>
                    <el-col :span='2' align='middle' style='line-height: 70px'>123/千支</el-col>
                    <el-col :span='3' align='middle' style='line-height: 70px'>10千支</el-col>
                    <el-col :span='4' align='middle'>
                        <el-row style='line-height: 35px'>
                            <span style='font-weight: bold'>243</span>
                            &nbsp;
                            <s style='font-size: 12px'>298</s>
                        </el-row>
                        <el-row style='line-height: 35px'>含运费<span>10.00</span></el-row>
                    </el-col>
                    <el-col :span='5.5' style='line-height: 80px;'>
                        <el-button type='primary'>订单确认</el-button>
                        <el-button type='info'>取消订单</el-button>
                    </el-col>
                </el-row>
            </div>
        </el-form>
    </el-main>

<!--    我的订单-->
    <el-main v-show='!orderValue'>
        <div style='margin: 5px auto;background-color: rgb(250,250,250);width: 100%;height: 50px;line-height: 50px;'>
            <el-breadcrumb separator="|" style=''>
                <el-breadcrumb-item :to="{ path: '/' }">全部</el-breadcrumb-item>
                <el-breadcrumb-item ><a href="/">合同待确认</a></el-breadcrumb-item>
                <el-breadcrumb-item >合同已确认</el-breadcrumb-item>
                <el-breadcrumb-item >待付款</el-breadcrumb-item>
                <el-breadcrumb-item >待发货</el-breadcrumb-item>
                <el-breadcrumb-item >待收货</el-breadcrumb-item>
                <el-breadcrumb-item >已发货</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <!-- 选择查询时间 -->
        <div class="selectTime">
            <span style='font-size:15px;margin:0 30px'>订单查询时间</span>
            <el-date-picker
                style='vertical-align: middle'
                v-model="value2"
                type="daterange"
                align="right"
                unlink-panels
                range-separator="至"
                start-placeholder="开始日期"
                end-placeholder="结束日期"
                :picker-options="pickerOptions">
            </el-date-picker>
            <el-input
                style='width: 30%;margin: 0 30px'
                placeholder="请输入关键词搜索订单"
                clearable>
            </el-input>
            <el-button type='primary' style='width: 100px'>搜索</el-button>
        </div>
<!--        查询条件及分页-->
        <div class='pagination'>
            <el-checkbox>合并付款</el-checkbox><el-checkbox>隐藏交易关闭订单</el-checkbox>
            <el-pagination
                style='float: right;margin-right: 30px;vertical-align: bottom'
                :page-size="10"
                :pager-count="3"
                layout="prev, pager, next"
                :total="80">
            </el-pagination>
        </div>
<!--        商品分类-->
        <el-row class="goodProp" >
            <el-col :span='8'>&nbsp;&nbsp;&nbsp;商品</el-col>
            <el-col :span='2' align='middle'>单价</el-col>
            <el-col :span='3' align='middle'>数量</el-col>
            <el-col :span='3' align='middle'>总金额</el-col>
            <el-col :span='4'>&nbsp;&nbsp;&nbsp;订单状态</el-col>
            <el-col :span='4'>交易操作</el-col>
        </el-row>
<!--        订单列表(一条订单信息)-->
        <div class='orderList'>
            <el-row class='orderListTop'>
                <el-col :span='23'>
                    <el-checkbox></el-checkbox>&nbsp;&nbsp;订单编号(非标订单)：
                    <span>3326522346465</span>
                </el-col>
                <el-col :span='1'>
                    <span @click='alert("确认删除？")'><i class="el-icon-delete" ></i></span>
                </el-col>
            </el-row>
<!--            订单信息-->
            <el-row class='orderListMain'>
                <el-col :span='2' align='middle' style='margin-left: 10px'>
                    <div style='line-height: 80px'>
                        商品图片
                    </div>
                </el-col>
                <el-col :span='6'>
                    <el-row style='line-height: 35px'><span style='font-weight: bold'>U形螺栓JB/ZQ 1234- -2000...</span></el-row>
                    <el-row style='line-height: 35px'>物料编号<span>115646541</span></el-row>
                </el-col>
                <el-col :span='2' align='middle' style='line-height: 70px'>123/千支</el-col>
                <el-col :span='3' align='middle' style='line-height: 70px'>10千支</el-col>
                <el-col :span='3' align='middle'>
                    <el-row style='line-height: 35px'>
                        <span style='font-weight: bold'>243</span>
                        &nbsp;
                        <s style='font-size: 12px'>298</s>
                    </el-row>
                    <el-row style='line-height: 35px'>含运费<span>10.00</span></el-row>
                </el-col>
                <el-col :span='4'>
                    <el-row style='line-height: 35px;color: #e6a23c'>合同待确认</el-row>
                    <el-row style='line-height: 35px'>
                        <el-col :span='9'><el-link>订单详情</el-link></el-col>
                        <el-col :span='9'><el-link>查看物流</el-link></el-col>
                    </el-row>
                </el-col>
                <el-col :span='3.5' style='line-height: 80px;'><el-button type='primary'>合同确认</el-button></el-col>
            </el-row>
        </div>
        <!--        订单列表(一条订单信息)-->
        <div class='orderList'>
            <el-row class='orderListTop'>
                <el-col :span='23'>
                    <el-checkbox></el-checkbox>&nbsp;&nbsp;订单编号(非标订单)：
                    <span>3326522346465</span>
                </el-col>
                <el-col :span='1'>
                    <span @click='alert("确认删除？")'><i class="el-icon-delete" ></i></span>
                </el-col>
            </el-row>
            <!--            订单信息-->
            <el-row class='orderListMain'>
                <el-col :span='2' align='middle' style='margin-left: 10px'>
                    <div style='line-height: 80px'>
                        商品图片
                    </div>
                </el-col>
                <el-col :span='6'>
                    <el-row style='line-height: 35px'><span style='font-weight: bold'>U形螺栓JB/ZQ 1234- -2000...</span></el-row>
                    <el-row style='line-height: 35px'>物料编号<span>115646541</span></el-row>
                </el-col>
                <el-col :span='2' align='middle' style='line-height: 70px'>123/千支</el-col>
                <el-col :span='3' align='middle' style='line-height: 70px'>10千支</el-col>
                <el-col :span='3' align='middle'>
                    <el-row style='line-height: 35px'>
                        <span style='font-weight: bold'>243</span>
                        &nbsp;
                        <s style='font-size: 12px'>298</s>
                    </el-row>
                    <el-row style='line-height: 35px'>含运费<span>10.00</span></el-row>
                </el-col>
                <el-col :span='4'>
                    <el-row style='line-height: 35px;color: #e6a23c'>合同待确认</el-row>
                    <el-row style='line-height: 35px'>
                        <el-col :span='9'><el-link>订单详情</el-link></el-col>
                        <el-col :span='9'><el-link>查看物流</el-link></el-col>
                    </el-row>
                </el-col>
                <el-col :span='3.5' style='line-height: 80px;'><el-button type='primary'>合同确认</el-button></el-col>
            </el-row>
        </div>
        <!--        订单列表(一条订单信息)-->
        <div class='orderList'>
            <el-row class='orderListTop'>
                <el-col :span='23'>
                    <el-checkbox></el-checkbox>&nbsp;&nbsp;订单编号(非标订单)：
                    <span>3326522346465</span>
                </el-col>
                <el-col :span='1'>
                    <span @click='alert("确认删除？")'><i class="el-icon-delete" ></i></span>
                </el-col>
            </el-row>
            <!--            订单信息-->
            <el-row class='orderListMain'>
                <el-col :span='2' align='middle' style='margin-left: 10px'>
                    <div style='line-height: 80px'>
                        商品图片
                    </div>
                </el-col>
                <el-col :span='6'>
                    <el-row style='line-height: 35px'><span style='font-weight: bold'>U形螺栓JB/ZQ 1234- -2000...</span></el-row>
                    <el-row style='line-height: 35px'>物料编号<span>115646541</span></el-row>
                </el-col>
                <el-col :span='2' align='middle' style='line-height: 70px'>123/千支</el-col>
                <el-col :span='3' align='middle' style='line-height: 70px'>10千支</el-col>
                <el-col :span='3' align='middle'>
                    <el-row style='line-height: 35px'>
                        <span style='font-weight: bold'>243</span>
                        &nbsp;
                        <s style='font-size: 12px'>298</s>
                    </el-row>
                    <el-row style='line-height: 35px'>含运费<span>10.00</span></el-row>
                </el-col>
                <el-col :span='4'>
                    <el-row style='line-height: 35px;color: #e6a23c'>合同待确认</el-row>
                    <el-row style='line-height: 35px'>
                        <el-col :span='9'><el-link>订单详情</el-link></el-col>
                        <el-col :span='9'><el-link>查看物流</el-link></el-col>
                    </el-row>
                </el-col>
                <el-col :span='3.5' style='line-height: 80px;'><el-button type='primary'>合同确认</el-button></el-col>
            </el-row>
        </div>
        <!--        订单列表(一条订单信息)-->
        <div class='orderList'>
            <el-row class='orderListTop'>
                <el-col :span='23'>
                    <el-checkbox></el-checkbox>&nbsp;&nbsp;订单编号(非标订单)：
                    <span>3326522346465</span>
                </el-col>
                <el-col :span='1'>
                    <span @click='alert("确认删除？")'><i class="el-icon-delete" ></i></span>
                </el-col>
            </el-row>
            <!--            订单信息-->
            <el-row class='orderListMain'>
                <el-col :span='2' align='middle' style='margin-left: 10px'>
                    <div style='line-height: 80px'>
                        商品图片
                    </div>
                </el-col>
                <el-col :span='6'>
                    <el-row style='line-height: 35px'><span style='font-weight: bold'>U形螺栓JB/ZQ 1234- -2000...</span></el-row>
                    <el-row style='line-height: 35px'>物料编号<span>115646541</span></el-row>
                </el-col>
                <el-col :span='2' align='middle' style='line-height: 70px'>123/千支</el-col>
                <el-col :span='3' align='middle' style='line-height: 70px'>10千支</el-col>
                <el-col :span='3' align='middle'>
                    <el-row style='line-height: 35px'>
                        <span style='font-weight: bold'>243</span>
                        &nbsp;
                        <s style='font-size: 12px'>298</s>
                    </el-row>
                    <el-row style='line-height: 35px'>含运费<span>10.00</span></el-row>
                </el-col>
                <el-col :span='4'>
                    <el-row style='line-height: 35px;color: #e6a23c'>合同待确认</el-row>
                    <el-row style='line-height: 35px'>
                        <el-col :span='9'><el-link>订单详情</el-link></el-col>
                        <el-col :span='9'><el-link>查看物流</el-link></el-col>
                    </el-row>
                </el-col>
                <el-col :span='3.5' style='line-height: 80px;'><el-button type='primary'>合同确认</el-button></el-col>
            </el-row>
        </div>
        <!--        订单列表(一条订单信息)-->
        <div class='orderList'>
            <el-row class='orderListTop'>
                <el-col :span='23'>
                    <el-checkbox></el-checkbox>&nbsp;&nbsp;订单编号(非标订单)：
                    <span>3326522346465</span>
                </el-col>
                <el-col :span='1'>
                    <span @click='alert("确认删除？")'><i class="el-icon-delete" ></i></span>
                </el-col>
            </el-row>
            <!--            订单信息-->
            <el-row class='orderListMain'>
                <el-col :span='2' align='middle' style='margin-left: 10px'>
                    <div style='line-height: 80px'>
                        商品图片
                    </div>
                </el-col>
                <el-col :span='6'>
                    <el-row style='line-height: 35px'><span style='font-weight: bold'>U形螺栓JB/ZQ 1234- -2000...</span></el-row>
                    <el-row style='line-height: 35px'>物料编号<span>115646541</span></el-row>
                </el-col>
                <el-col :span='2' align='middle' style='line-height: 70px'>123/千支</el-col>
                <el-col :span='3' align='middle' style='line-height: 70px'>10千支</el-col>
                <el-col :span='3' align='middle'>
                    <el-row style='line-height: 35px'>
                        <span style='font-weight: bold'>243</span>
                        &nbsp;
                        <s style='font-size: 12px'>298</s>
                    </el-row>
                    <el-row style='line-height: 35px'>含运费<span>10.00</span></el-row>
                </el-col>
                <el-col :span='4'>
                    <el-row style='line-height: 35px;color: #e6a23c'>合同待确认</el-row>
                    <el-row style='line-height: 35px'>
                        <el-col :span='9'><el-link>订单详情</el-link></el-col>
                        <el-col :span='9'><el-link>查看物流</el-link></el-col>
                    </el-row>
                </el-col>
                <el-col :span='3.5' style='line-height: 80px;'><el-button type='primary'>合同确认</el-button></el-col>
            </el-row>
        </div>
    </el-main>
</el-container>
</el-container>

</template>
<script>
export default {
    data() {
        const item = {
        };
        return {
            orderValue:true,            //“我的订单”、“创建订单”页面显示切换
            dialogTableVisible:false,   //弹窗显示切换
            activeStep:0,   //步骤进度
            tabPosition: 'left',
            radio1:'1',     //种类选择
            radio2:'1',     //强度
            radio3:'1',     //表面处理
            num: 1,     //箱数
            thousands:0.32,     //千支数
            orderCreate:true,  //订单创建页面的显示
            orderExplain:false,     //待分析页面的显示
            priceExplain:false,  //待报价页面的显示
            orderConfirm:false,  //待确认页面的显示
            strengthNumberALL:[true,false,false,false,false],     //各强度参数展示
            options: [{     //表面处理种类
                value: '镀锌',
                label: '镀锌'
            }, {
                value: '达克罗（锌铬涂层）',
                label: '达克罗（锌铬涂层）'
            }, {
                value: '久美特（锌铝涂层）',
                label: '久美特（锌铝涂层）'
            }, {
                value: '发黑',
                label: '发黑'
            }, {
                value: '镀铬',
                label: '镀铬'
            }, {
                value: '表面处理禁用',
                label: '表面处理禁用',
                disabled: true
            }],
            ffValue: '',   //表面处理选择
            options2: [{    //材质种类
                value: '低镍665',
                label: '低镍665'
            }, {
                value: '钛合金TB3',
                label: '钛合金TB3'
            }, {
                value: 'ML42CrMo',
                label: 'ML42CrMo'
            }, {
                value: '435CRMO',
                label: '435CRMO'
            }, {
                value: 'A3',
                label: 'A3'
            }],
            materialValue:'', //材质选择
            tableData: Array(5).fill(item),
            pickerOptions: {
                shortcuts: [{
                    text: '最近一周',
                    onClick(picker) {
                        const end = new Date();
                        const start = new Date();
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                        picker.$emit('pick', [start, end]);
                    }
                }, {
                    text: '最近一个月',
                    onClick(picker) {
                        const end = new Date();
                        const start = new Date();
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                        picker.$emit('pick', [start, end]);
                    }
                }, {
                    text: '最近三个月',
                    onClick(picker) {
                        const end = new Date();
                        const start = new Date();
                        start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                        picker.$emit('pick', [start, end]);
                    }
                }]
            },
            value1: '',
            value2: '',
        }
    },
    methods:{
        showCreateOrder(){
            this.orderValue=true;
        },
        showMyOrder(){
            this.orderValue=false;
        },
        handleChange(value) {
            console.log(value);
        },
        //提交之后返回界面
        stepNext(){
            //1.关闭弹框
            this.dialogTableVisible=false;
            //2.步骤进行到下一步
            // if(this.activeStep++ > 5){
            //     this.activeStep = 5;
            // };
            this.activeStep = 4
            //3.页面切换
            this.orderCreate=false; //隐藏订单创建页
           // this.orderExplain=true; //显示待分析页
            this.orderConfirm=true; //显示待确认页
        },
        stepNext2(){
            //1.关闭弹框
            this.dialogTableVisible=false;
            //2.步骤进行到下一步
            this.activeStep++;
            if(this.activeStep++ > 5){
                this.activeStep = 5;
            };
            //3.页面切换
            this.orderCreate=false; //隐藏订单创建页
            this.orderExplain=false; //隐藏待分析页
            this.priceExplain=true;   //显示待报价页面
        },

        strengthChoose1(){
            for(let i=0,len=this.strengthNumberALL.length;i<=len;i++){
                if(i!=0){
                    this.strengthNumberALL[i]=false;
                }else{
                    this.strengthNumberALL[i]=true;
                }
            }
        },
        strengthChoose2(){
            for(let i=0,len=this.strengthNumberALL.length;i<=len;i++){
                if(i!=1){
                    this.strengthNumberALL[i]=false;
                }else{
                    this.strengthNumberALL[i]=true;
                }
            }
        },
        strengthChoose3(){
            for(let i=0,len=this.strengthNumberALL.length;i<=len;i++){
                if(i!=2){
                    this.strengthNumberALL[i]=false;
                }else{
                    this.strengthNumberALL[i]=true;
                }
            }
        },
        strengthChoose4(){
            for(let i=0,len=this.strengthNumberALL.length;i<=len;i++){
                if(i!=3){
                    this.strengthNumberALL[i]=false;
                }else{
                    this.strengthNumberALL[i]=true;
                }
            }
        },
        strengthChoose5(){
            for(let i=0,len=this.strengthNumberALL.length;i<=len;i++){
                if(i!=4){
                    this.strengthNumberALL[i]=false;
                }else{
                    this.strengthNumberALL[i]=true;
                }
            }
        }
    }
};
</script>
<style>

.activeClass{
    font-size: 18px;
    color: rgb(19,143,255);
    background-color: rgb(230,244,255);
}
.menuTitle{
    height: 56px;
    line-height: 50px;
    margin-left: 20px;
    font-weight: bold;
    font-size: 20px;

}
.el-header {
    width: 100%;
    background-color: rgb(247,247,247);
    color: #333;
    height: 60px;
    line-height: 60px;
    margin-left:10px;
    padding-left:10px;
    text-align: center;
    font-weight: bold;
}
.col1{
    width:100px;
}
.col2{
    width:100px;
    float: right;
    margin-right: 20px;
}
.el-main {
    width: 100%;
    margin-left: 10px;
    background-color: white;
    font-family: "Microsoft YaHei";
}
.el-breadcrumb{
    text-align:center;
    height: 60px;
    line-height: 60px;
}
.selectTime{
    display: inline-block;
    text-align: left;
    margin: 5px auto;
    width: 100%;
    height: 50px;
    line-height: 50px;
    border: 1px solid rgb(240,240,240);

}
.pagination{
    display: inline-block;
    text-align: left;
    margin: 5px auto;
    width: 100%;
    height: 40px;
    line-height: 40px;
}
.goodProp{
    display: inline-block;
    text-align: left;
    margin: 5px auto;
    width: 100%;
    height: 40px;
    line-height: 40px;
    background-color: rgb(246,246,246);
    border: 1px solid rgb(162, 162, 162);
    font-size: 15px;
    color: gray;
}
.orderList{
    text-align: left;
    margin: 5px auto;
    width: 100%;
    color: gray;
}
.orderListTop{
    height: 30px;
    line-height: 30px;
    background-color: rgb(247,247,247);
    color: rgb(122,122,122);

}
.orderListMain{
    border: 1px solid rgb(162, 162, 162);
    border-top: none;
    height: 80px;
    font-size: 15px;
}
.el-button{
    margin-right: 0;
    padding-left: -10px;
    width: 100px;
}
.el-aside {
    color: #333;
}
.el-dialog__header{
    display: none;
}
.el-dialog{
    margin:0 auto;
    padding-top: 5px;
    width: 350px;
    text-align: center;
}
.priceExplain-css{
    width:350px;
    height: 250px;
    margin:0 auto;
    text-align: center;
}
</style>

