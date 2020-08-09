<template>
  <div class="app-container">
  
    <el-form ref="form" :model="form" label-width="120px">
      <el-form-item align="right">
        <el-button  type="primary" @click="onSubmit">审核</el-button>
        <el-button @click="onCancel">关闭</el-button>
      </el-form-item>
      <el-form-item label="收银单号">
        <el-input v-model="saleorder.payOrderMainId" />
      </el-form-item>
      
    </el-form>

     <el-table
      v-loading="listLoading"
      :data="saleorder.paryOrderDetail"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      
    >
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index+1 }}
        </template>
      </el-table-column>
       <el-table-column label="型号" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.model }}
        </template>
      </el-table-column>
       <el-table-column label="数量" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.qty }}
        </template>
      </el-table-column>
       <el-table-column label="标价" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.listPrice }}
        </template>
      </el-table-column>
       <el-table-column label="折扣" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.discount }}
        </template>
      </el-table-column>
         <el-table-column label="净价" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.netPrice }}
        </template>
      </el-table-column>
     </el-table>

      <el-table
      v-loading="listLoading"
      :data="saleorder.paryOrderPayment"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      
    >
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index+1 }}
        </template>
      </el-table-column>
       <el-table-column label="支付方式" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.payType }}
        </template>
      </el-table-column>
       <el-table-column label="支付时间" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.payDate }}
        </template>
      </el-table-column> 
       <el-table-column label="支付金额" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.payAmt }}
        </template> 
        </el-table-column>
         <el-table-column label="支付链接" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.payQrcode }}
        </template> 
      </el-table-column>
     </el-table>
       <el-table
      v-loading="listLoading"
      :data="saleorder.pictureInfo"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      
    >
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index+1 }}
        </template>
      </el-table-column>
       <el-table-column label="支付方式" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.payType }}
        </template>
      </el-table-column>
       <el-table-column label="支付时间" width="120" align="center">
        <template slot-scope="scope">
          {{ scope.row.payDate }}
        </template>
      </el-table-column>
     </el-table>
    <!-- <el-form ref="form" :model="form" label-width="120px">
      <el-form-item label="Activity name">form
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="Activity zone">
        <el-select v-model="form.region" placeholder="please select your zone">
          <el-option label="Zone one" value="shanghai" />
          <el-option label="Zone two" value="beijing" />
        </el-select>
      </el-form-item>
      <el-form-item label="Activity time">
        <el-col :span="11">
          <el-date-picker
            v-model="form.date1"
            type="date"
            placeholder="Pick a date"
            style="width: 100%;"
          />
        </el-col>
        <el-col :span="2" class="line">-</el-col>
        <el-col :span="11">
          <el-time-picker
            v-model="form.date2"
            type="fixed-time"
            placeholder="Pick a time"
            style="width: 100%;"
          />
        </el-col>
      </el-form-item>
      <el-form-item label="Instant delivery">
        <el-switch v-model="form.delivery" />
      </el-form-item>
      <el-form-item label="Activity type">
        <el-checkbox-group v-model="form.type">
          <el-checkbox label="Online activities" name="type" />
          <el-checkbox label="Promotion activities" name="type" />
          <el-checkbox label="Offline activities" name="type" />
          <el-checkbox label="Simple brand exposure" name="type" />
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="Resources">
        <el-radio-group v-model="form.resource">
          <el-radio label="Sponsor" />
          <el-radio label="Venue" />
        </el-radio-group>
      </el-form-item>
      <el-form-item label="Activity form">
        <el-input v-model="form.desc" type="textarea" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">Create</el-button>
        <el-button @click="onCancel">Cancel</el-button>
      </el-form-item>
    </el-form> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      saleorder: {
       
      }
    };
  },
  created() {
    this.fetchDataById();
  },
  methods: {
    onSubmit() {
      this.$message("submit!");
    },
    onCancel() {
      this.$message({
        message: "cancel!",

        type: "warning"
      });
    },
    fetchDataById() {
      var id = this.$route.params.id;
      var vm = this;
      this.axios({
        method: "GET",
        url:"http://172.30.107.42:9999/order/qryOrderDetailByOrderIdGet?qryInfo=" +id
      }).then(function(resp) {
        console.log(resp.data);
        vm.saleorder = resp.data.data;
      });
    }
  }
};
</script>

<style scoped>
.line {
  text-align: center;
}
</style>
