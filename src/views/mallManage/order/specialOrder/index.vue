<template>
  <el-container>
    <el-header height='120px'>
      <h2>普通订单</h2>
      <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleClick">
        <el-menu-item index="total">全部</el-menu-item>
        <el-menu-item index="contractToBeConfirmed">合同待确认</el-menu-item>
        <el-menu-item index="toBePaid">待付款</el-menu-item>
        <el-menu-item index="inProduction">生产中</el-menu-item>
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
        <el-table-column label="操作" width="150" align="left">
          <template slot-scope="scope">
            <a style="color: #2d8cf0"
               size="mini"
               @click="handleViewOrder(scope.$index, scope.row)"
            >查看详情</a>
            <a style="color: #2d8cf0"
               size="mini"
               @click='contractConfirmDialog = true'
               v-show="scope.row.orderStatus==='合同待确认'">合同确认</a>
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
    <!--        合同确认弹出层-->
    <el-dialog title="合同确认" :visible.sync="contractConfirmDialog"
               class='contractConfirm'  width="720px">
      <div class='orderInfo'>
        <el-row>
          <el-col>订单基本信息（非标订单）</el-col>
        </el-row>
        <el-row>
          <el-col :span='10'>
            <strong>订单编号：</strong>
            <span>13148295632</span>
          </el-col>
          <el-col :span='12'>
            <strong>订单金额：</strong>￥
            <span>234.00</span>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span='10'>
            <strong>客户名称：</strong>
            <span>申亿五金标准件有限公司</span>
          </el-col>
          <el-col :span='12'>
            <strong>订单产品：</strong>
            <span>***螺栓加防腐处理等</span>
          </el-col>
        </el-row>
      </div>
      <el-main>
        <!--    我方合同上传   -->
        <div >
          <p>我方合同上传</p>
          <el-upload
            action="#"
            list-type="picture-card"
            :auto-upload="false">
            <i slot="default" class="el-icon-plus"></i>
            <div slot="file" slot-scope="{file}">
              <img
                class="el-upload-list__item-thumbnail"
                :src="file.url" alt=""
              >
              <span class="el-upload-list__item-actions">
                             <!-- 查看 -->
                             <span
                               class="el-upload-list__item-preview"
                               @click="handlePictureCardPreview(file)"
                             >
                                <i class="el-icon-zoom-in"></i>
                             </span>
                <!-- 下载 -->
                             <span
                               v-if="!disabled"
                               class="el-upload-list__item-delete"
                               @click="handleDownload(file)"
                             >
                               <i class="el-icon-download"></i>
                             </span>
                <!-- 删除-->
                             <span
                               v-if="!disabled"
                               class="el-upload-list__item-delete"
                               @click="handleRemove(file)"
                             >
                               <i class="el-icon-delete"></i>
                             </span>
                      </span>
            </div>
          </el-upload>
          <!--   我方合同大图显示    -->
          <el-dialog :visible.sync="dialogVisible" z-index='1'>
            <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
        </div>

        <br />

        <!--    客户合同查看   -->
        <div >
          <p>客户合同查看</p>
          <el-upload
            action="#"
            list-type="picture-card"
            :auto-upload="false">
            <i slot="default" class="el-icon-plus"></i>
            <div slot="file" slot-scope="{file}">
              <img
                class="el-upload-list__item-thumbnail"
                :src="file.url" alt=""
              >
              <span class="el-upload-list__item-actions">
                             <!-- 查看 -->
                             <span
                               class="el-upload-list__item-preview"
                               @click="handlePictureCardPreview(file)"
                             >
                                <i class="el-icon-zoom-in"></i>
                             </span>
                <!-- 下载 -->
                             <span
                               v-if="!disabled"
                               class="el-upload-list__item-delete"
                               @click="handleDownload(file)"
                             >
                               <i class="el-icon-download"></i>
                             </span>
                <!-- 删除-->
                             <span
                               v-if="!disabled"
                               class="el-upload-list__item-delete"
                               @click="handleRemove(file)"
                             >
                               <i class="el-icon-delete"></i>
                             </span>
                      </span>
            </div>
          </el-upload>
          <!--    客户合同大图显示    -->
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
        </div>
        <br /><p>合同确认(信息确认无误后将生成购物订单)</p><br />

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
        <!--    确定和取消   -->
        <div style='text-align: right;margin-right: 20px'>
          <el-button type="primary">确定</el-button>
          <el-button @click='contractConfirmDialog = false'>取消</el-button>
        </div>
      </el-main>
    </el-dialog>
  </el-container>
</template>

<script>
export default {
  name: 'specialOrderList',
  data() {
    return {
      activeIndex: 'toBeDelivered',
      multipleSelection: [],
      listLoading: false,
      timeValue: '',
      contractConfirmDialog:false,    //合同确认弹出框
      dialogImageUrl: '', //下面三个都是上传文件组件自带的数值
      dialogVisible: false,
      info:null,     //判断信息是否正确
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
          id:1,
          orderNumber:"42530002",
          customerName:"广东**有限公司",
          phone:12345546,
          orderStatus:'合同待确认',
          orderDate:'2021/02/06 18:33:45'
        },
        {
          id:1,
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
      this.$router.push({path:'/mallManage/order/specialOrderDetail',query:{id:row.id}})
    },
    //文件上传的一些相关方法
    handleRemove(file) {
      console.log(file);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleDownload(file) {
      console.log(file);
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
