<template>
  <el-container>
    <el-header height='120px'>
      <h2>普通订单</h2>
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleClick">
        <el-menu-item index="total">全部</el-menu-item>
        <el-menu-item index="toBePaid">待付款</el-menu-item>
        <el-menu-item index="toBeDelivered">待发货</el-menu-item>
        <el-menu-item index="toBeReceived">待收货</el-menu-item>
        <el-menu-item index="received">已收货</el-menu-item>
      </el-menu>
    </el-header>
    <el-card class="box-card">
      <el-row class='orderSearch'>
        <span style='margin:0 40px;font-size: 16px'>订单查询</span>
        <el-date-picker
          v-model="timeValue"
          type="datetimerange"
          size='1'
          align="right"
          unlink-panels
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :picker-options="pickerOptions">
        </el-date-picker>
        <el-button size='1' type='primary' style='margin-left: 20px'>查询</el-button>
        <el-button size='1' type='primary' style='float: right;margin: 5px 20px 5px 0;'>查询</el-button>
        <el-input size='1' clearable placeholder="请输入关键字查询订单" style='width: 25%;float: right;margin-right: 30px;'></el-input>
      </el-row>
    </el-card>
    <div class="table-container">
      <el-table ref="orderTable"
                :data="allOrder"
                style="width: 100%;"
                v-loading="listLoading" border>
        <el-table-column label="序号" width="80" align="center">
          <template slot-scope="scope">{{scope.row.id}}</template>
        </el-table-column>
        <el-table-column label="订单编号" width="400" align="center">
          <template slot-scope="scope">{{scope.row.orderNumber}}</template>
        </el-table-column>
        <el-table-column label="客户名称" width="120" align="center">
          <template slot-scope="scope">{{scope.row.customerName}}</template>
        </el-table-column>
        <el-table-column label="手机号码" width="120" align="center">
          <template slot-scope="scope">{{scope.row.phone}}</template>
        </el-table-column>
        <el-table-column label="订单状态" align="center">
          <template slot-scope="scope">{{scope.row.orderStatus}}</template>
        </el-table-column>
        <el-table-column label="订单日期" align="center">
          <template slot-scope="scope">{{scope.row.orderDate}}</template>
        </el-table-column>
        <el-table-column label="操作" width="300" align="left">
          <template slot-scope="scope">
            <a style="color: #2d8cf0"
               size="mini"
               @click="handleViewOrder(scope.$index, scope.row)"
            >查看详情</a>
          </template>
        </el-table-column>
      </el-table>
      <!--        分页      -->
      <el-pagination
        background
        style='float: right;'
        layout="prev, pager, next"
        :total="100">
      </el-pagination>
    </div>
  </el-container>
</template>

<script>
export default {
  name: 'ordinaryOrderList',
  data() {
    return {
      activeIndex: 'toBeDelivered',
      multipleSelection: [],
      listLoading: false,
      timeValue: '',
      pickerOptions: {
        shortcuts: [{
          text: '本月',
          onClick(picker) {
            picker.$emit('pick', [new Date(), new Date()]);
          }
        }, {
          text: '今年至今',
          onClick(picker) {
            const end = new Date();
            const start = new Date(new Date().getFullYear(), 0);
            picker.$emit('pick', [start, end]);
          }
        }, {
          text: '最近六个月',
          onClick(picker) {
            const end = new Date();
            const start = new Date();
            start.setMonth(start.getMonth() - 6);
            picker.$emit('pick', [start, end]);
          }
        }]
      },
      allOrder:[
        {
          id:1,
          orderNumber:"42530002",
          customerName:"**有限公司",
          phone:12345546,
          orderStatus:'待付款',
          orderDate:'2021/02/06 18:33:45'
        },
        {
          id:2,
          orderNumber:"42530002",
          customerName:"广东**有限公司",
          phone:12345546,
          orderStatus:'待收货',
          orderDate:'2021/02/06 18:33:45'
        },
        {
          id:3,
          orderNumber:"42530002",
          customerName:"个**有限公司",
          phone:12345546,
          orderStatus:'待评价',
          orderDate:'2021/02/06 18:33:45'
        },
      ]

    }
  },
  created() {
  },
  methods:{
    handleViewOrder(index, row){
      this.$router.push({path:'/mallManage/order/ordinaryOrderDetail',query:{id:row.id}})
    },
    handleClick(tab, event) {
      console.log(tab, event);
    }
  }

};
</script>

<style scoped>
.orderStatus span{
  margin:0 20px;
  cursor: pointer;    /*放上去边手型*/
}
.orderSearch{
  width: 100%;
  height: 50px;
  line-height: 50px;
  margin-top: 10px;
  margin-bottom: 10px;
  font-weight: bold;
}
</style>
