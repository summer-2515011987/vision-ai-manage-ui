<template>
  <div class="userManagement">
    <createBtn titleText="用户管理" />
    <el-card>
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
        <el-button type="primary" @click="handleAdd()" class="addBtn"
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
        <el-table-column label="角色名称" prop="name" align="center">
          <template slot-scope="scope">{{ scope.row.name }}</template>
        </el-table-column>
        <el-table-column label="描述" align="center">
          <template slot-scope="scope">{{ scope.row.description }}</template>
        </el-table-column>
        <el-table-column label="添加时间" align="center">
          <template slot-scope="scope">{{ scope.row.createTime }}</template>
        </el-table-column>
        <el-table-column label="是否启用" width="100px" align="center">
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
              @click="handleSelectMenu(scope.$index, scope.row)"
              >分配菜单
            </el-button>
            <el-button
              size="mini"
              @click="handleSelectResource(scope.$index, scope.row)"
              >分配资源
            </el-button>

            <el-button
              size="mini"
              @click="handleUpdate(scope.$index, scope.row)"
              >编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)"
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
        :total="1000"
      >
      </el-pagination>
    </el-card>
  </div>
</template>

<script>
import createBtn from "@/components/createBtn";
const defaultListQuery = {
  pageNum: 1,
  pageSize: 10,
  keyword: null
};
export default {
  name: "userManagement",
  components: {
    createBtn
  },
  data() {
    return {
      listQuery: Object.assign({}, defaultListQuery),
      listLoading: false,
      list: [{ name: "zs" }]
    };
  },
  created() {},
  mounted() {},
  methods: {
    handleResetSearch() {
      this.listQuery = Object.assign({}, defaultListQuery);
    },
    getList() {
      console.log("获取列表数据");
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
</style>
