<template>
  <el-container>
    <el-header height='30%'>
      <h2>开票管理</h2>
      <el-row style='width:100%;text-align: center;padding:5px'>
        <el-input style='width: 500px;'
                  size='1'
                  placeholder='请输入关键字搜索'
        >
        </el-input>
        <el-button type='primary' size='1'>搜索</el-button>
      </el-row>
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleClick">
        <el-menu-item index="total">全部</el-menu-item>
        <el-menu-item index="applied">已申请</el-menu-item>
        <el-menu-item index="notInvoiced">未开票</el-menu-item>
        <el-menu-item index="invoiced">已开票</el-menu-item>
      </el-menu>
    </el-header>
    <div class="table-container">
      <el-table ref="orderTable"
                :data="allUserList"
                style="width: 100%;"
                v-loading="listLoading" border>
        <el-table-column label="序号" width="80" align="center">
          <template slot-scope="scope">{{scope.row.id}}</template>
        </el-table-column>
        <el-table-column label="用户名称" align="center">
          <template slot-scope="scope">{{scope.row.username}}</template>
        </el-table-column>
        <el-table-column label="订单详情" width="120" align="center">
          <template slot-scope="scope">
          <a style="color: #2d8cf0"
             size="mini"
             @click="handleViewOrder(scope.$index, scope.row)"
          >查看详情</a>
          </template>
        </el-table-column>
        <el-table-column label="开票状态" width="120" align="center">
          <template slot-scope="scope">{{scope.row.phone}}</template>
        </el-table-column>
        <el-table-column label="操作" width="160" align="left">
          <template slot-scope="scope">
            <a style="color: #2d8cf0"
               size="mini"
               @click="handleBilling(scope.$index, scope.row)"
            >开票</a>
            &nbsp;&nbsp;
            <a style="color: #2d8cf0"
               size="mini"
               @click="handleSetLogistics(scope.$index, scope.row)"
            >设置物流</a>
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
    <!--        设置付款弹出层-->
    <el-dialog
      title="开票"
      :visible.sync="dialogVisible" width="30%">
      <el-form :model="billing"
               ref="billingForm" label-width="100px">
        <el-form-item label="发票类型：">
          <span style="color: #1e6abc;">{{billing.type}}</span>
        </el-form-item>
        <el-form-item label="公司名称：">
          <span style="font-weight:bold">{{billing.name}}</span>
        </el-form-item>
        <el-form-item label="纳税人号：">
          <span style="font-weight:bold">{{billing.id}}</span>
        </el-form-item>
        <el-form-item label="手机号码：">
          <span style="font-weight:bold">{{billing.phone}}</span>
        </el-form-item>
        <el-form-item label="寄送地址：">
          <span style="font-weight:bold">{{billing.address}}</span>
        </el-form-item>
        <el-form-item label="物流单号：">
          <el-input v-model="billing.deliverySn" class="input-width"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="handleConfirm">确 定</el-button>
      </span>
    </el-dialog>
  </el-container>
</template>

<script>
export default {
  name: 'invoiceList',
  data() {
    return {
      activeIndex: 'invoiced',
      listLoading: false,
      isShow:true,
      payTypeList:["按期付款"],  //选择支付方式
      dialogVisible:false,
      billing:{
        type:"增值税普通发票",
        name:"申亿五金标准件有限公司",
        id:"12312312313",
        phone:"18292001029",
        address:"张三 18292001029 湖南省长沙市天心区",
        deliverySn:""
      },
      allUserList:[       //所有用户
        {
          id:1,
          username:"个人客户",
          status:0,
          userType:"个人认证",
          phone:12345546,
          agent:'无',
        },
        {
          id:2,
          username:"待审核客户",
          status:2,
          userType:"待审核",
          phone:1555231,
          agent:'无',
        },
        {
          id:3,
          username:"企业客户",
          status:1,
          userType:"企业认证",
          phone:133331,
          agent:'代理人',
        },
        {
          id:1,
          username:"审核未通过客户",
          status:3,
          userType:"个人认证",
          phone:12345546,
          agent:'无',
        }
      ],
    };
  },
  methods: {
    handleViewOrder(index, row){
      this.$router.push({path:'/mallManage/order/invoiceDetail',query:{id:row.id}})
    },
    handleBilling(index, row){
      this.dialogVisible = true;
    },
    handleSetLogistics(index, row){
      this.dialogVisible = true;
    },
    handleConfirm(){
      this.dialogVisible = false;
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
