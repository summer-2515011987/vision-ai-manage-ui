<template>
  <div class="userManagement">
    <createBtn titleText="用户管理" />
    <el-card class="userCard">
      <!-- 表单项 -->
      <el-form
        :inline="true"
        :model="listQuery"
        class="demo-form-inline"
        style="text-align:left;"
      >
        <el-form-item label="">
          <el-input
            v-model="listQuery.keyword"
            class="input-width"
            placeholder="角色名称"
            clearable
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-button
            type="primary"
            @click="handleSearchList()"
            class="searchBtn"
            >查询搜索</el-button
          >
          <el-button
            type="primary"
            @click="handleResetSearch()"
            class="searchBtn"
            >重置</el-button
          >
        </el-form-item>
        <el-button type="primary" @click="handleAdd({})" class="addBtn"
          >添加</el-button
        >
      </el-form>
      <!-- 列表项 -->
      <el-table
        ref="roleTable"
        header-row-class-name="light_grey_table"
        :data="list"
        class="tableList"
        height="400px"
        v-loading="listLoading"
        stripe
        border
      >
        <el-table-column
          label="账号"
          prop="account"
          align="center"
          width="100px"
        >
          <template slot-scope="scope">{{ scope.row.account }}</template>
        </el-table-column>
        <el-table-column label="姓名" prop="name" align="center" width="80px">
          <template slot-scope="scope">{{ scope.row.name }}</template>
        </el-table-column>
        <el-table-column label="邮箱" prop="email" align="center">
          <template slot-scope="scope">{{ scope.row.email }}</template>
        </el-table-column>
        <el-table-column label="添加时间" align="center" prop="createTime">
          <template slot-scope="scope">{{ scope.row.createTime }}</template>
        </el-table-column>
        <el-table-column label="最后登陆" align="center" prop="endTime">
          <template slot-scope="scope">{{ scope.row.endTime }}</template>
        </el-table-column>
        <el-table-column
          label="是否启用"
          width="100px"
          align="center"
          prop="status"
        >
          <template slot-scope="scope">
            <el-switch
              @change="handleStatusChange(scope.$index, scope.row)"
              :active-value="1"
              :inactive-value="0"
              v-model="scope.row.status"
            >
            </el-switch>
          </template>
        </el-table-column>
        <el-table-column label="操作" width="400px" align="center">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="allocationQuota(scope.$index, scope.row)"
              >分配配额
            </el-button>
            <el-button size="mini" @click="assignRoles(scope.$index, scope.row)"
              >分配角色
            </el-button>
            <el-button size="mini" @click="handleAdd(scope.row)"
              >编辑
            </el-button>
            <el-button size="mini" type="danger" @click="deleteItem(scope.row)"
              >删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        class="footer-pagination"
        background
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        layout="total, sizes,prev, pager, next,jumper"
        :current-page.sync="listQuery.pageNum"
        :page-size="10"
        :page-sizes="[10, 15, 20]"
        :total="total"
      >
      </el-pagination>
    </el-card>
    <!-- 使用组件 -->
    <addUser
      :visible="detailState"
      :detail="detail"
      @cancel="detailState = false"
    ></addUser>
    <assignRoles
      :roleVisible="roleVisible"
      @cancel="roleVisible = false"
    ></assignRoles>
    <!-- 分配配额 -->
    <allocationQuota
      :alloVisible="alloVisible"
      @cancel="alloVisible = false"
    ></allocationQuota>
  </div>
</template>

<script>
// 导入删除接口（以下接口仅为模拟，需根据实际状况导入)
// import {
//     delApplicationModule
//   } from "@/api/systemManage/sysAppliationModule";
import createBtn from "@/components/createBtn";
import addUser from "./components/addUser.vue"; //添加用户
import assignRoles from "./components/assignRoles.vue";
import allocationQuota from "./components/allocationQuota.vue";
const defaultListQuery = {
  pageNum: 1,
  pageSize: 10,
  keyword: null
};
export default {
  name: "userManagement",
  components: {
    createBtn,
    addUser,
    assignRoles,
    allocationQuota
  },
  data() {
    return {
      total: 10,
      detail: {},
      detailState: false, //添加用户
      roleVisible: false, //角色
      alloVisible: false, //分配配额
      listQuery: Object.assign({}, defaultListQuery), //传到列表里的参数
      listLoading: false,
      list: [
        {
          id: 1,
          account: "251501111",
          name: "zs",
          email: "2515011987@qq.com",
          createTime: "2021-06-15",
          endTime: "2021-06-17",
          status: 1
        },
        {
          id: 2,
          account: "251501222",
          name: "ls",
          email: "2515011444@qq.com",
          createTime: "2021-06-15",
          endTime: "2021-06-17",
          status: 0
        },
        {
          id: 3,
          account: "251501222",
          name: "ls",
          email: "2515011444@qq.com",
          createTime: "2021-06-15",
          endTime: "2021-06-17",
          status: 0
        },
        {
          id: 4,
          account: "251501222",
          name: "ww",
          email: "2515011555@qq.com",
          createTime: "2021-06-12",
          endTime: "2021-06-17",
          status: 1
        }
      ]
    };
  },
  created() {
    this.getList();
  },
  mounted() {},
  methods: {
    // 查询列表
    getList() {
      console.log("获取列表数据");
      // this.listLoading = true;
      // fetchList(this.listQuery).then(response => {
      //   this.listLoading = false;
      //   this.list = response.data.list;
      //   this.total = response.data.total;
      // });
    },
    // 是否启用
    handleStatusChange(index, row) {
      this.$confirm("是否要修改该状态?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          // 调接口传参
          // updateStatus(row.id, { status: row.status }).then(response => {
          //   this.$message({
          //     type: "success",
          //     message: "修改成功!"
          //   });
          // });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "取消修改"
          });
          this.getList();
        });
    },
    // 分配配额
    allocationQuota() {
      this.alloVisible = true;
    },
    // 分配角色
    assignRoles() {
      console.log("111");
      this.roleVisible = true;
    },
    // 删除用户
    deleteItem(data) {
      console.log("删除", data);
      // let params = {
      //   id: data.id
      // };
      // this.$confirm(`确定要删除模块【${data.name}】吗？`, "删除系统应用模块", {
      //   confirmButtonText: "确定",
      //   cancelButtonText: "取消",
      //   type: "warning"
      // }).then(() => {
      //   delApplicationModule(params)
      //     .then(res => {
      //       let data = res.data;
      //       if (data.resultCode === 200) {
      //         this.$message.success(data.data);
      //         this.changePage(1);
      //       } else {
      //         this.$message.error(`删除失败`);
      //       }
      //     })
      //     .catch(err => {
      //       this.$message.error(err.response.data.msg);
      //     });
      // });
    },
    //添加编辑用户
    handleAdd(data) {
      this.detail = JSON.parse(JSON.stringify(data));
      this.detailState = true;
    },
    handleResetSearch() {
      this.listQuery = Object.assign({}, defaultListQuery);
    },

    handleSearchList() {
      this.listQuery.pageNum = 1;
      this.getList();
    },
    handleSizeChange(val) {
      console.log("handleSizeChange");
      this.listQuery.pageNum = 1;
      this.listQuery.pageSize = val;
      this.getList();
    },
    handleCurrentChange(val) {
      console.log("handleCurrentChange");
      this.listQuery.pageNum = val;
      this.getList();
    }
  }
};
</script>

<style scoped lang="less">
.addBtn {
  float: right;
}
.tableList {
  margin-top: 10px;
}
.userCard {
  margin-top: 20px;
}
</style>
