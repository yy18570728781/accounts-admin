<template>
  
  <el-dialog title="总结明细" :visible="DetDialog_" center width="98%"  @close="closeEdit">
    <el-table
      v-loading="listLoading"
      :data="temList"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      ref="filterTable"
      :default-sort="{ prop: 'userName', order: 'descending' }"
    >

      <el-table-column
        label="会员总结"
        align="center"
      >
        
        <el-table-column
          label="会员 ID"
          align="center"
          prop="userCode"
          sort-by="userCode"
          sortable
        >

          <template slot-scope="scope">
            <span v-if="scope.row.userCode">{{scope.row.userCode}} </span>
            <span v-else style="fpnt-size:20px">总计</span>
          </template>

        </el-table-column>
        <el-table-column
          label="会员名"
          align="center"
          prop="userName"
        >
        </el-table-column>
        <el-table-column
          label="流水"
          align="center"
          prop="turnover"
          sort-by="turnover"
          sortable
        >
        </el-table-column>


        <el-table-column
          label="玩家利润提成"
          align="center"
          prop="playerProfitBonus"
          sort-by="playerProfitBonus"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="流水提成"
          align="center"
          prop="turnoverBonus"
          sort-by="turnoverBonus"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="利润提成"
          align="center"
          prop="profitBonus"
          sort-by="profitBonus"
          sortable
        >
        </el-table-column>

        <el-table-column
          label="钱包"
          align="center"
          prop="wallet"
          sort-by="wallet"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="输赢"
          align="center"
          prop="winLose"
          sort-by="winLose"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="玩家总结"
          align="center"
          prop="playerProfit"
          sort-by="playerProfit"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="总结"
          align="center"
          prop="profit"
          sort-by="profit"
          sortable
        >
        </el-table-column>

      </el-table-column>
        
    </el-table>
    <div class="page">
      <el-pagination 
        @size-change="handleSizeChange" 
        @current-change="handleCurrentChange" 
        :current-page="currentPage" 
        :page-sizes="pageSizes" 
        :page-size="PageSize" layout="total, sizes, prev, pager, next, jumper" 
        :total="totalCount">
      </el-pagination>
    </div>
    <div style="height:50px;"></div>
    <el-table
      v-loading="listLoading"
      :data="temList1"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      ref="filterTable"
      :default-sort="{ prop: 'userName', order: 'descending' }"
    >
      <el-table-column
        label="代理总结"
        align="center"
      >

        <el-table-column
        label="代理 ID"
        align="center"
        prop="userCode"
        sort-by="userCode"
        sortable
      >
        <template slot-scope="scope">
          <span v-if="scope.row.userCode">{{scope.row.userCode}} </span>
          <span v-else style="fpnt-size:20px">总计</span>
        </template>
      </el-table-column>
      <el-table-column
        label="代理名"
        align="center"
        prop="userName"
      >
      </el-table-column>
      <el-table-column
        label="流水"
        align="center"
        prop="turnover"
        sort-by="turnover"
        sortable
      >
      </el-table-column>


      <el-table-column
        label="代理提成"
        align="center"
        prop=""
        sort-by=""
      >

        <el-table-column
          label="流水提成"
          align="center"
          prop="agentTurnoverBonus"
          sort-by="agentTurnoverBonus"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="利润提成"
          align="center"
          prop="agentProfitBonus"
          sort-by="agentProfitBonus"
          sortable
        >
        </el-table-column>

      </el-table-column>
      <el-table-column
        label="提成"
        align="center"
        prop=""
        sort-by=""
      >

        <el-table-column
          label="流水提成"
          align="center"
          prop="turnoverBonus"
          sort-by="turnoverBonus"
          sortable
        >
        </el-table-column>
        <el-table-column
          label="利润提成"
          align="center"
          prop="profitBonus"
          sort-by="profitBonus"
          sortable
        >
        </el-table-column>

      </el-table-column>

      <el-table-column
        label="钱包"
        align="center"
        prop="wallet"
        sort-by="wallet"
        sortable
      >
      </el-table-column>
      <el-table-column
        label="输赢"
        align="center"
        prop="winLose"
        sort-by="winLose"
        sortable
      >
      </el-table-column>
      <el-table-column
        label="代理总结"
        align="center"
        prop="agentProfit"
        sort-by="agentProfit"
        sortable
      >
      </el-table-column>
      <el-table-column
        label="总结"
        align="center"
        prop="profit"
        sort-by="profit"
        sortable
      >
      </el-table-column>

      </el-table-column>
      
    </el-table>
    <div class="page">
      <el-pagination 
        @size-change="handleSizeChange1" 
        @current-change="handleCurrentChange1" 
        :current-page="currentPage1" 
        :page-sizes="pageSizes1" 
        :page-size="PageSize1" layout="total, sizes, prev, pager, next, jumper" 
        :total="totalCount1">
      </el-pagination>
    </div>
  </el-dialog>
</template>
<script>
import { getDetAgentPlayerSum , getDetAgentSum } from "@/api/member";
export default {
  name: "Detail",
  props:{
    DetDialog:{
      type:Boolean,
      default:false
    },
    gnuserId:{
      type:String
    },
    fromDate:{
      type:String
    },
    toDate:{
      type:String
    },
  },
  data() {
    return {
      listLoading: false,
      memberList: [],//会员列表
      agencypList: [],//代理列表

      detailShow:false,//明细开关

      // 分页
      // 总数据
      memberList: [],
      // 展示数据
      temList:[],
      // 默认显示第几页
      currentPage:1,
      // 总条数，根据接口获取数据长度(注意：这里不能为空)
      totalCount:1,
      // 个数选择器（可修改）
      pageSizes:[5,10,20,30],
      // 默认每页显示的条数（可修改）
      PageSize:10,
      count:{},//总计

      // 分页1
      // 总数据
      agencypList: [],
      // 展示数据
      temList1:[],
      // 默认显示第几页
      currentPage1:1,
      // 总条数，根据接口获取数据长度(注意：这里不能为空)
      totalCount1:1,
      // 个数选择器（可修改）
      pageSizes1:[5,10,20,30],
      // 默认每页显示的条数（可修改）
      PageSize1:10,
      count1:{},//总计
    };
  },
  created(){
    
  },
  mounted(){
    // console.log(this.DetDialog,this.gnuserId,this.fromDate,this.toDate);
  },
  components: {},
  computed:{
    DetDialog_: {
      get() {
        return this.DetDialog;
      },
      set(val) {
        //grants_改变由父组件控制
        this.$emit("changeDetDialog", val);
      }
    },
  },
  methods: {
    // 关闭弹窗
    closeEdit() {
      this.$emit("changeDetDialog", false);
    },

    //每页显示的条数
    handleSizeChange(val) {
        // 改变每页显示的条数 
        this.PageSize=val
        // 注意：在改变每页显示的条数时，要将页码显示到第一页
        this.currentPage=1
    },
    //显示第几页
    handleCurrentChange(val) {
      console.log(val,'val');
        //改变默认的页数
        this.currentPage=val
        this.getTemList()
        console.log(this.currentPage,'this.curpage');
    },
    getTemList(){
      this.temList =  this.memberList.slice((this.currentPage-1)*this.PageSize,this.currentPage*this.PageSize)
      this.temList.push(this.count)
    },
  // --------------------------------------------------
    //每页显示的条数
    handleSizeChange1(val) {
        // 改变每页显示的条数 
        this.PageSize1=val
        // 注意：在改变每页显示的条数时，要将页码显示到第一页
        this.currentPage1=1
    },
    //显示第几页
    handleCurrentChange1(val) {
      console.log(val,'val');
        //改变默认的页数
        this.currentPage1=val
        this.getTemList1()
        console.log(this.currentPage1,'this.curpage');
    },
    getTemList1(){
      this.temList1 =  this.agencypList.slice((this.currentPage1-1)*this.PageSize1,this.currentPage1*this.PageSize1)
      this.temList1.push(this.count1)
    },

    getList(gnuserId,fromDate,toDate) {
      console.log(this.DetDialog,this.gnuserId,this.fromDate,this.toDate);
      if (this.fromDate && this.toDate) {
        this.listLoading = true;
        getDetAgentPlayerSum({ gnuserId:gnuserId, fromDate:fromDate, toDate:toDate })
          .then((res) => {
            console.log(res,'会员总结');
            this.memberList = res.data;

            let playerProfitBonus = 0;
            let turnoverBonus = 0;
            let profitBonus = 0;
            let turnover = 0;
            let winLose = 0;
            let wallet = 0;
            let playerProfit = 0;
            let profit = 0;
            this.memberList.forEach(item=>{
              playerProfitBonus += Number(item.playerProfitBonus)
              turnoverBonus += Number(item.turnoverBonus)
              profitBonus += Number(item.profitBonus)
              turnover += Number(item.turnover)
              winLose += Number(item.winLose)
              wallet += Number(item.wallet)
              playerProfit += Number(item.playerProfit)
              profit += Number(item.profit)
            })
            playerProfitBonus = Number(playerProfitBonus).toFixed(2)
            turnoverBonus = Number(turnoverBonus).toFixed(2)
            profitBonus = Number(profitBonus).toFixed(2)
            turnover = Number(turnover).toFixed(2)
            winLose = Number(winLose).toFixed(2)
            wallet = Number(wallet).toFixed(2)
            playerProfit = Number(playerProfit).toFixed(2)
            profit = Number(profit).toFixed(2)
            this.count = { playerProfitBonus,turnoverBonus,profitBonus, turnover,winLose,wallet,playerProfit,profit}
            this.count.firstColumn = '总计' 
            this.getTemList()

            this.listLoading = false;
          })
          .catch((err) => {
            this.listLoading = false;
            console.log(err);
          });

        getDetAgentSum({ gnuserId:gnuserId, fromDate:fromDate, toDate:toDate })
          .then((res) => {
            console.log(res,'代理总结');
            this.agencypList = res.data;

            let agentTurnoverBonus = 0;
            let turnoverBonus = 0;
            let agentProfitBonus = 0;
            let profitBonus = 0;
            let agentProfit = 0;
            let winLose = 0;
            let wallet = 0;
            let profit = 0;
            let turnover = 0;
            this.agencypList.forEach(item=>{
              agentTurnoverBonus += Number(item.agentTurnoverBonus)
              turnoverBonus += Number(item.turnoverBonus)
              agentProfitBonus += Number(item.agentProfitBonus)
              profitBonus += Number(item.profitBonus)
              agentProfit += Number(item.agentProfit)
              winLose += Number(item.winLose)
              wallet += Number(item.wallet)
              profit += Number(item.profit)
              turnover += Number(item.turnover)
            })
            agentTurnoverBonus = Number(agentTurnoverBonus).toFixed(2)
            turnoverBonus = Number(turnoverBonus).toFixed(2)
            agentProfitBonus = Number(agentProfitBonus).toFixed(2)
            profitBonus = Number(profitBonus).toFixed(2)
            agentProfit = Number(agentProfit).toFixed(2)
            winLose = Number(winLose).toFixed(2)
            wallet = Number(wallet).toFixed(2)
            profit = Number(profit).toFixed(2)
            turnover = Number(turnover).toFixed(2)
            this.count1 = { agentTurnoverBonus,turnoverBonus,agentProfitBonus, profitBonus,agentProfit,winLose,wallet,profit,turnover}
            this.count.firstColumn = '总计' 
            this.getTemList1()

            this.listLoading = false;
          })
          .catch((err) => {
            this.listLoading = false;
            console.log(err);
          });
          
      } else {
        this.$message({ type: "info", message: "Please select time" });
      }
    },
  },
};
</script>
<style lang="scss" scoped>

.flex-box {
  display: flex;
  flex-wrap: wrap;
  .item {
    margin-right: 10px;
    margin-top: 10px;
    margin-bottom: 10px;
  }
  .el-select{
    width: 250px !important;
  }
  .el-input__inner{
    width: 250px !important;
  }
  ::v-deep .el-table td{
    padding: 0 !important;
  }
}
</style>