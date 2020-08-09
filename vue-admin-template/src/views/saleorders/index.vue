<template>
  <el-container>
    <el-header height="90px">
      <div class="app-container">
     
          <el-input
            v-model="listQuery.title"
            placeholder="用户姓名、手机号"
            style="width: 200px;"
            class="filter-item"
            @keyup.enter.native="handleFilter"
          />
          
          <!-- <el-select
            v-model="listQuery.importance"
            placeholder="门店"
            clearable
            style="width: 140px ;margin-left: 10px;"
            class="filter-item"
          >
            <el-option
              v-for="item in list"
              :key="item"
              :label="item"
              :value="item"
            />
          </el-select>
          <el-select
            v-model="listQuery.type"
            placeholder="直销员"
            clearable
            class="filter-item;"
            style="width: 130px;margin-left: 10px;"
          >
            <el-option
              v-for="item in list"
              :key="item.key"
              :label="item.display_name + '(' + item.key + ')'"
              :value="item.key"
            />
          </el-select>
          <el-select
            v-model="listQuery.type"
            placeholder="收银单状态"
            clearable
            class="filter-item "
            style="width: 130px;margin-left: 10px;"
          >
            <el-option
              v-for="item in list"
              :key="item.key"
              :label="item.display_name + '(' + item.key + ')'"
              :value="item.key"
            />
          </el-select> -->
          <el-date-picker
            v-model="value1"
            type="date"
            placeholder="开始日期"
            style="width:250px;margin-left: 10px;"
          />
         
          ---
       
          <el-date-picker
            v-model="value1"
            type="date"
            placeholder="结束日期"
            style="width: 250px;margin-left: 10px;"
          />
          <template align="right">
            <el-button
            
              class="filter-item"
               style="margin-left: 10px;"
              type="primary"
              icon="el-icon-search"
              align="right"
          
            >
              查询
            </el-button>
            <el-button
              class="filter-item"
              style="margin-left: 10px;"
              type="primary"
              icon="el-icon-edit"
            >
              审核
            </el-button>
            <el-button
              
              :loading="downloadLoading"
              class="filter-item"
              type="primary"
              icon="el-icon-download"
            >
              导出
            </el-button>
          </template>
        </div>
 
    </el-header>
    <el-main>
      <el-table
        :data="list"
        border
        fit
        highlight-current-row
        style="width: 100%;"
      >
        <el-table-column align="center" label="序号" width="95">
          <template slot-scope="scope">
            {{ scope.$index + 1 }}
          </template>
        </el-table-column>
        <el-table-column label="创建日期" width="300" align="center">
          <template slot-scope="scope">
            {{ scope.row.createDate }}
          </template>
        </el-table-column>
        <el-table-column label="收银单号" width="300" align="center">
          <template slot-scope="scope">
            {{ scope.row.payOrderMainId }}
          </template>
        </el-table-column>
        <el-table-column label="开票客户" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.billtoCode }}
          </template>
        </el-table-column>
        <el-table-column label="直销员姓名" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.empName }}
          </template>
        </el-table-column>
        <el-table-column label="报单门店" width="200" align="center">
          <template slot-scope="scope">
            {{ scope.row.storeName }}
          </template>
        </el-table-column>
        <el-table-column label="用户姓名" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.customerName }}
          </template>
        </el-table-column>
        <el-table-column label="联系电话" width="110" align="center">
          <template slot-scope="scope">
            {{ scope.row.customerMobile }}
          </template>
        </el-table-column>
        <el-table-column label="订单金额" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.orderAmt }}
          </template>
        </el-table-column>
        <el-table-column label="总应付金额" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.payAmt }}
          </template>
        </el-table-column>
        <el-table-column label="本次待支付金额" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.toPayAmt }}
          </template>
        </el-table-column>
        <el-table-column label="总计实付金额" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.actualPayAmt }}
          </template>
        </el-table-column>
        <el-table-column label="收银单状态" width="100" align="center">
          <template slot-scope="scope">
            {{ scope.row.orderStatus }}
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          align="center"
          width="230"
          class-name="small-padding fixed-width"
        >
          <template slot-scope="scope">
            <el-button
              type="primary"
              size="mini"
              align="right"
              @click="editorder(scope.row.payOrderMainId)"
            >
              编辑
            </el-button>

            <el-button
              v-if="scope.row.orderStatus != 0"
              size="mini"
              type="danger"
              @click="auditorder(scope.row.payOrderMainId)"
            >
              审核
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-main>

    <pagination
      v-show="total > 0"
      :total="total"
      :page.sync="listQuery.page"
      :limit.sync="listQuery.limit"
      @pagination="getList"
    />
  </el-container>
</template>

<script>
import { getList } from "@/api/saleorders";
import Pagination from "@/components/Pagination"; // secondary package based on el-pagination
export default {
  components: { Pagination },
  data() {
    return {
      list: [
        {
          payOrderMainId: "00ea57adc41348baa425e950855dd270",
          billtoCode: "HNJCY",
          orderCode: "7",
          empId: "7BE6589678384958A845AD51A5069B07",
          empName: "徐东",
          storeId: "D702E69D49F749DD84DF6373FE6C25DE",
          storeCode: null,
          storeName: "空净南京苏宁电器新街口店",
          customerUnionid: "123456",
          customerMobile: "15850746700",
          customerName: "BMTEST2018072401",
          customerCityId: "532",
          customerCityName: "南京市",
          customerCountyId: "",
          customerCountyName: "南京市__DSADSAD",
          customerAddress: "",
          orderAmt: 2323,
          discountAmt: 0,
          payAmt: 2323,
          actualPayAmt: 2323,
          toPayAmt: 0,
          remark: null,
          orderStatus: 2,
          createDate: "2020-08-01T04:00:05.877+00:00",
          updateDate: "2020-08-01T04:00:05.877+00:00",
          salesManId: null,
          salesManName: null,
          assistantName: null,
          orderSource: null,
          payQrcode: null,
          totalQty: 0,
          totalAmt: 0,
          paryOrderDetail: [
            {
              payOrderDetailId: "0a6caf97ec7240659158462b2783e9ed",
              payOrderMainId: "00ea57adc41348baa425e950855dd270",
              fourCode: "a154",
              model: "PF25C1",
              qty: 1,
              listPrice: 0,
              discount: 0,
              netPrice: 2323
            }
          ],
          paryOrderPayment: [
            {
              paymentId: "1ec5c687b1054ddcbc413bc751588ca2",
              payOrderMainId: "00ea57adc41348baa425e950855dd270",
              payType: 1,
              payOrderCode: "7",
              payAmt: 2323,
              payQrcode:
                "https://qr.95516.com/48020000/4591202008011158288084870472",
              createDate: "2020-08-01T04:00:05.890+00:00",
              payDate: "2020-08-01T04:00:05.890+00:00",
              payStatus: "1"
            }
          ],
          pictureInfo: []
        },
        {
          payOrderMainId: "c058a2fdd3d24947be8569fb294049a6",
          billtoCode: "HNJCY",
          orderCode: "C902F6D897F00001F7D31600FB5017B1",
          empId: "72CB733D9235431F8541A511C8BA37B5",
          empName: "石祖锋",
          storeId: "0895FD2114CE40F29429B0D1D9E7A914",
          storeCode: "B03154",
          storeName: "南京迈皋桥红太阳",
          customerUnionid: "",
          customerMobile: "17714530606",
          customerName: "2个产品",
          customerCityId: "532",
          customerCityName: "南京市",
          customerCountyId: "8AB71F6D-F53F-E511-80B9-B8CA3A47871C",
          customerCountyName: "秦淮区",
          customerAddress: "南京市秦淮区地址地址",
          orderAmt: 240.0,
          discountAmt: 0.0,
          payAmt: 240.0,
          actualPayAmt: 240.0,
          toPayAmt: 0.0,
          remark: "备注了吗",
          orderStatus: 4,
          createDate: "2020-08-07T02:30:15.550+00:00",
          updateDate: "2020-08-07T02:30:15.550+00:00",
          salesManId: "28CC6B1DB677402892357AD3ECEE4D67",
          salesManName: "安桂兰--南京苏宁河西店",
          assistantName: "带单人",
          orderSource: "展会带单",
          payQrcode: "",
          totalQty: 0,
          totalAmt: 0.0,
          paryOrderDetail: [
            {
              payOrderDetailId: "1f17b6aef8174f4f8374cf01188a03e5",
              payOrderMainId: "c058a2fdd3d24947be8569fb294049a6",
              fourCode: "a154",
              model: "CEWH-100PG6",
              qty: 2,
              listPrice: 20.0,
              discount: 0.0,
              netPrice: 20.0
            },
            {
              payOrderDetailId: "9f965ecf619249a59e0c82b00fbd1068",
              payOrderMainId: "c058a2fdd3d24947be8569fb294049a6",
              fourCode: "wadr",
              model: "PF25C1",
              qty: 2,
              listPrice: 100.0,
              discount: 0.0,
              netPrice: 100.0
            }
          ],
          paryOrderPayment: [
            {
              paymentId: "ff67fd86d13f47a48024eca5e7294582",
              payOrderMainId: "c058a2fdd3d24947be8569fb294049a6",
              payType: 2,
              payOrderCode: "C902F6D897F00001F7D31600FB5017B1",
              payAmt: 240.0,
              payQrcode: "",
              createDate: "2020-08-07T02:30:15.573+00:00",
              payDate: null,
              payStatus: "1"
            }
          ],
          pictureInfo: []
        }
      ],
      list2: null,
      listLoading: true,
      downloadLoading:false,
      value1: "",
      total: 0,
      listQuery: {
        page: 1,
        limit: 20,
        importance: undefined,
        title: undefined,
        type: undefined,
        sort: "+id"
      }
    };
  },
  created() {
    //this.fetchData();
  },
  methods: {
    // fetchData() {
    //   this.listLoading = true;
    //   getList().then(response => {
    //     this.list = response.data.items;
    //     this.listLoading = false;
    //   });
    // },
    getList1() {
      //获取数据
      this.listLoading = true;
      fetchList(this.listQuery).then(response => {
        this.list = response.data.items;
        this.total = response.data.total;

        // Just to simulate the time of the request
        setTimeout(() => {
          this.listLoading = false;
        }, 1.5 * 1000);
      });
    },
    getList() {
      //获取数据
      var vm = this;
      this.axios({
        method: "GET",
        url:
          "http://localhost:8090/product/pageinfo?pageNum=" +
          vm.listQuery.page +
          "&pageSize=" +
          vm.listQuery.limit
      }).then(function(resp) {
        //得到一个pageinfo对象
        vm.dotal = resp.data.total;
        vm.list = resp.data.list;
      });
    },
    fetchData() {
      // var id =this.$route.params.id;
      var vm = this;
      var id = "00ea57adc41348baa425e950855dd270";
      this.axios({
        method: "GET",
        url:
          "http://172.30.107.42:9999/order/qryOrderDetailByOrderIdGet?qryInfo=" +
          id
      }).then(function(resp) {
        vm.list = resp.data;
      });
    },
    editorder(id) {
      this.$router.push("/editsaleorder/index/" + id);
    },
    auditorder(id) {
      var vm = this;
      this.axios({})
        .then(function(resp) {
          //弹窗
          if (resp.data.data == "success") {
            vm.$message({
              message: "审核成功!",
              type: "success"
            });
            vm.fetchData(); //更新订单列表
          } else {
            vm.$message({
              message: "审核失败!",
              type: "error"
            });
          }
        })
        .catch(function(error) {
          vm.$message.error("审核失败");
        });
    }
  }
};
</script>
