<template>
  <div class="work-order-details">
    <el-card class="order-card">
      <h4 class="title">
        <span class="desc">订单详情</span>
      </h4>
      <el-form label-width="150px" ref="form" :model="formData" v-loading="detailsLoading">
        <el-row>
          <el-col :md="8" :sm="12">
            <el-form-item label="线别:">
              <el-input v-model="formData.factory" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="发单时间:">
               <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.orderCreateDate">
                <el-date-picker
                  disabled
                  slot="reference"
                  v-model="formData.orderCreateDate"
                  type="datetime">
                </el-date-picker>
              </el-popover>
              
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="生产订单号:">
              <el-input v-model="formData.workOrderNumber" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="序号:">
              <el-input v-model="formData.sequenceNumber" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="P0号:" prop="p0">
              <el-input v-model="formData.p0Number" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="K/3单据编号:" prop="deliveryOrderNumber">
              <el-input v-model="formData.deliveryOrderNumber" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="客户名称:">
               <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.customerName">
                <el-input slot="reference" v-model="formData.customerName" disabled></el-input>
              </el-popover>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="K/3料号:">
              <el-input v-model="formData.productCode" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="品名:">
              <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.configCode">
                <el-input slot="reference" v-model="formData.configCode" disabled></el-input>
              </el-popover>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="型号:">
              <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.productSpec">
                <el-input slot="reference" v-model="formData.productSpec" disabled></el-input>
              </el-popover>
            </el-form-item>
          </el-col>
            <el-col :md="8" :sm="12">
            <el-form-item label="数量:" prop="quantity">
              <el-input v-model="formData.quantity" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="标识:">
              <el-input v-model="formData.processIdentity" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="配置sim卡:">
              <el-input v-model="configureSim" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="应用程序:" prop="appBom">
              <el-input v-model="formData.appBom" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="定制程序:">
              <el-input v-model="formData.customBom" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="内核版本:">
              <el-input v-model="formData.kernelBom" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="单位:">
              <el-input v-model="formData.units" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="收货方:">
              <el-input v-model="formData.consignee" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="到货地:">
              <el-input v-model="formData.consigneeAddress" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="发货时间:">
              <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.deliveryDate">
                <el-date-picker 
                  slot="reference" 
                  type="datetime" 
                  v-model="formData.deliveryDate" 
                  disabled>
                </el-date-picker>
              </el-popover>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="准成品料号:">
              <el-input v-model="formData.productBom" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="版本号:">
              <el-input v-model="formData.version" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="机身号起始:">
              <el-input v-model="formData.snBegin" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="机身号结束:">
              <el-input v-model="formData.snEnd" disabled></el-input>
            </el-form-item>
          </el-col>
           <el-col :md="8" :sm="12">
            <el-form-item label="TUSN起始:">
              <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.tusnBegin">
                <el-input slot="reference" v-model="formData.tusnBegin" disabled></el-input>
              </el-popover>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="TUSN结束:">
              <el-popover
                placement="top-start"
                :width="contentWidth"
                trigger="hover"
                :content="formData.tusnEnd">
                <el-input slot="reference" v-model="formData.tusnEnd" disabled></el-input>
              </el-popover>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="银联标贴起始流水码:" prop="unionPayStickersBegin">
              <el-input v-model="formData.unionpayLabelStart" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="银联标贴结束流水码:" prop="unionPayStickersEnd">
              <el-input v-model="formData.unionpayLabelEnd" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="起始序列号:" prop="serialNumberBegin">
              <el-input v-model="formData.sequenceNumberStart" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="结束序列号:" prop="serialNumberEnd">
              <el-input v-model="formData.sequenceNumberEnd" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="boot版本号:" prop="targetVersionBoot">
              <el-input v-model="formData.targetVersionBoot" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="recovery版本号:" prop="targetVersionRecovery">
              <el-input v-model="formData.targetVersionRecovery" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="core版本号:" prop="targetVersionCore">
              <el-input v-model="formData.targetVersionCore" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="test版本号:" prop="targetVersionTest">
              <el-input v-model="formData.targetVersionTest" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="app版本号:" prop="targetVersionApp">
              <el-input v-model="formData.targetVersionApp" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="wifi版本号:" prop="targetVersionWifi">
              <el-input v-model="formData.targetVersionWifi" disabled></el-input>
            </el-form-item>
          </el-col>
          <el-col :md="8" :sm="12">
            <el-form-item label="无线版本号:" prop="targetVersionWireless">
              <el-input v-model="formData.targetVersionWireless" disabled></el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-form-item class="el-col el-col-24" label="工单备注:" prop="orderRemark">
            <el-input v-model="formData.orderRemark" disabled type="textarea" :autosize="{ minRows: 4, maxRows: 6}"></el-input>
          </el-form-item>
        </el-row>
        <el-button v-if="orderState === 5 && $checkBtnPermission('order.check')" type="primary" class="purple-btn" @click="checkOrder(orderId)">通过</el-button>
        <el-button v-if="orderState === 5 && $checkBtnPermission('order.abandon')" type="danger" @click="abandonOrder(orderId)">废弃</el-button>
      </el-form>
    </el-card>
  </div>
</template>
<script>
import { simStates } from '@/utils/dictionary'
import { getOrderDetails, checkOrder, abandonOrder } from '@/api/order'
import { getAllFactory } from '@/api/factory'
export default {
  name: '',
  components: {},
  props: {},
  directive: {},
  data() {
    return {
      contentWidth: 240,
      simStates,
      factoryList: [],
      detailsLoading: false,
      orderState: 5,
      formData: {
        workOrderNumber: '', // 生产订单号
        orderCreateDate: '', // 发单时间
        deliveryOrderNumber: '', // 出货单号
        p0Number: '', // p0号
        quantity: '', // 数量
        productSpec: '', // 型号
        configCode: '', // 品名
        processIdentity: 0, // 标识
        appBom: '', // 应用程序物料编号
        kernelBom: '', // 内核版本物料编号
        customerName: '', // 客户名称
        deliveryDate: '', // 发货时间
        consignee: '', // 收货方
        consigneeAddress: '', // 收货地址
        snBegin: '', // 产品SN起
        snEnd: '', // 产品SN止
        orderRemark: '', // 工单备注
        orderImportDate: '', // 投入时间
        orderStartDate: '', // 生产时间
        orderCompleteDate: '', // 结案时间
        factory: '', // 线别
        foreignIdentity: '', // 国内外标识，21号文下载标识
        targetVersionBoot: '', // 目标boot版本号
        targetVersionRecovery: '', // 目标recovery版本号
        targetVersionCore: '', // 目标core版本号
        targetVersionTest: '', // 目标test版本号
        targetVersionApp: '', // 目标app版本号
        targetVersionWifi: '', // 目标wifi版本号
        targetVersionWireless: '', // 目标无线版本号
        id: '', // 工单id
        tusnBegin: '', // tusn起始
        tusnEnd: '', // tusn结束
        unionpayLabelStart: '', // 银联标贴开始流水码
        unionpayLabelEnd: '', // 银联标贴结束流水码
        sequenceNumber: '', // 序号
        sequenceNumberStart: '', // 起始序列号
        sequenceNumberEnd: '', // 结束序列号
        version: '', // 版本号
        units: '', // 单位
        customBom: '', // 定制程序
        configureSim: '', // 配置sim卡
        productCode: '', // K/3料号
        productBom: '', // 准成品料号
      }
    }
  },
  computed: {
    orderId() {
      return this.$route.params.orderId
    },
    configureSim() {
      let configureSim = this.formData.configureSim
      if (configureSim == 1) {
        return '是'
      } else {
        return '否'
      }
    },
  },
  watch: {},
  created() {
    this.orderState = _.toNumber(localStorage.getItem('orderState'))
    this.getOrderDetails()
    this.getAllFactory()
  },
  beforeMount() {},
  mounted() {
},
  beforeDestroy() {},
  destroyed() {
    localStorage.removeItem('orderState')
  },
  methods: {
    getAllFactory() {
      getAllFactory().then(res => {
        this.factoryList = res.data ? res.data.data : []
      })
    },  
    getOrderDetails() {
      this.detailsLoading = true
      getOrderDetails(this.orderId).then(res => {
        const resData = res.data.data
        this.formData = resData
        this.formData.orderCreateDate = this.formatTime(this.formData.orderCreateDate)
        this.formData.deliveryDate = this.formatTime(this.formData.deliveryDate)
        this.detailsLoading = false
      }).catch(err => {
        this.detailsLoading = false
      })
    },
    formatTime(time) {
      return moment(time).format('YYYY-MM-DD HH:mm:ss')
    },
    checkOrder(id) {
      checkOrder({id}).then(res => {
        this.$message.success('订单审核通过')
        this.$router.push('../list')
      }).catch((err) => {
        console.log(err)
      })
    },
     /* 废弃订单 */
    abandonOrder(id) {
      this.$confirm(`此操作将废弃订单，是否继续?`, '提示', {
        confirmButtonText:'确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        abandonOrder({id}).then(res => {
          this.$message.success('废弃订单成功')
          this.$router.push('../list')
        })
      }).catch(() => {
        console.log('取消废弃订单')
      })
    },
    handleCancel() {
      this.$store.dispatch('delView', this.$route)
      this.$router.push('../list')
    }
  }
}
</script>
<style lang="scss">
.el-textarea.is-disabled .el-textarea__inner,
.el-input.is-disabled .el-input__inner{
  text-overflow: ellipsis;
}
</style>
<style lang='scss' scoped>
@import "@/styles/variables.scss";
.work-order-details{
  background-color: $color_fdfdfd;
  .order-card{
    box-shadow: none;
    width: 80%;
    margin: auto;
    .el-card__body{
      padding: 20px 50px;
    }
  }
  .el-form{
    width: 96%;
    margin: auto;
    padding: 30px 50px;
    text-align: center;
  }
  .title{
    .desc{
      padding-left: 10px;
      border-left: 4px solid $color_6B77ED;
    }
  }
}

/* 媒体查询优化小分辨率样式 */
@media screen and (max-width:1680px) {
  .work-order-details{
  .order-card{
      width: 90%;
      .el-card__body{
        padding: 20px 30px;
      }
      .el-form{
        width: 96%;
        padding: 30px 20px;
      }
    }
  }
}
@media screen and (max-width:1600px) {
  .work-order-details{
  .order-card{
      width: 92%;
      .el-card__body{
        padding: 20px 30px;
      }
      .el-form{
        width: 96%;
        padding: 30px 10px;
      }
    }
  }
}
@media screen and (max-width:1440px) {
  .work-order-details{
  .order-card{
      width: 92%;
      .el-card__body{
        padding: 20px 30px;
      }
      .el-form{
        width: 100%;
        padding: 30px 0;
      }
    }
  }
}
</style>
