<template>
  <el-container>
    <el-header height='120px'>
      <h2>普通订单</h2>
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleClick">
        <el-menu-item index="total">全部</el-menu-item>
        <el-menu-item index="notReviewed">未审核</el-menu-item>
        <el-menu-item index="reviewed">已审核</el-menu-item>
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
                :data="allUserList"
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
        <el-table-column label="操作" width="160" align="left">
          <template slot-scope="scope">
            <a style="color: #2d8cf0"
               size="mini"
               @click='checkDialog = true'
            >审核</a>
            &nbsp;&nbsp;
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
    <!--   审核    -->
    <el-dialog :visible.sync="checkDialog" title='审核'>
      <div class='orderInfo'>
        <el-row>
          <el-col>订单基本信息（非标订单）</el-col>
        </el-row>
        <el-row>
          <el-col :span='12'>
            <strong>订单编号：</strong>
            <span>13148295632</span>
          </el-col>
          <el-col :span='12'>
            <strong>订单金额：</strong>￥
            <span>234.00</span>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span='12'>
            <strong>客户名称：</strong>
            <span>申亿五金标准件有限公司</span>
          </el-col>
          <el-col :span='12'>
            <strong>订单产品：</strong>
            <span>***螺栓加防腐处理等</span>
          </el-col>
        </el-row>
      </div>
      <el-row style='font-size: 16px'>
        <el-col :span='12'>
          <span style='color: #8c939d'>订单支付凭证</span>
        </el-col>
        <el-col :span='12' style='text-align: right'>
          <i class='el-icon-zoom-in'></i>&nbsp;
          <i class='el-icon-zoom-out'></i>&nbsp;
          <i class='el-icon-refresh-left'></i>&nbsp;
        </el-col>
      </el-row>
      <el-image name="voucher" src="https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg" width='800px' height='550px'></el-image>
      <br/> <br/>
      <!--信息结果的确认-->
      <div>
        <el-row style='height:32px;line-height:32px'>
          <el-col :span='10'>
            <b style='color: #2d8cf0;'>结果确认:</b>&nbsp;&nbsp;
            <span><el-radio v-model="info" label='1'>信息无误</el-radio ></span>
          </el-col>
          <el-col :span='14' style='text-align: right'>
            <el-radio v-model="info" label='0'>信息有误</el-radio>
            <el-input style='width: 250px' placeholder='输入原因进行反馈'></el-input>
          </el-col>
        </el-row>
      </div>
      <el-divider></el-divider>
      <!--    确定和取消   -->
      <div style='text-align: right;margin-right: 20px'>
        <el-button type="primary">确定</el-button>
        <el-button @click='checkDialog = false'>取消</el-button>
      </div>
    </el-dialog>
  </el-container>
</template>

<script>
export default {
  name: 'customerList',
  data() {
    return {
      checkDialog: false, // 控制审核dialog的隐藏/显示
      info:1,     //判断信息是否正确
      activeIndex: 'notReviewed',
      timeValue: '',
      listLoading: false,
      pickerOptions: {    //el选择日期组件自带
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
      allUserList:[
        {
          id:1,
          orderNumber:"42530002",
          customerName:"**有限公司",
          phone:12345546,
          orderStatus:'待审核',
          orderDate:'2021/02/06 18:33:45'
        },
        {
          id:1,
          orderNumber:"42530002",
          customerName:"广东**有限公司",
          phone:12345546,
          orderStatus:'已审核',
          orderDate:'2021/02/06 18:33:45'
        },
        {
          id:1,
          orderNumber:"42530002",
          customerName:"个**有限公司",
          phone:12345546,
          orderStatus:'待审核',
          orderDate:'2021/02/06 18:33:45'
        },
      ],
    };
  },
  methods: {
    handleViewOrder(index, row){
      this.$router.push({path:'/mallManage/order/payCheckOrderDetail',query:{id:row.id}})
    },
    handleClick(tab, event) {
      console.log(tab, event);
    },
  }

}
</script>
<style scoped>
.customerType .el-col{
  cursor: pointer;    /*放上去边手型*/
}
.customerType .colActive{
  border-bottom: 2px solid #2d8cf0;
  color: #2d8cf0;
}
.el-dialog__wrapper >>> .el-dialog__header{
  border-bottom: 1px solid #e8eaec;
  font-size: 16px;
  font-weight: bold;
}
.el-dialog__wrapper >>> .el-dialog__body{
  padding: 20px;
}
</style>
