<template>
  <div class="users-ct">
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-card>
      <el-row :gutter="20">
        <el-col :span="7">
          <el-input placeholder="请输入内容">
            <el-button slot="append" icon="el-icon-search"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <el-button type="primary">添加用户</el-button>
        </el-col>
      </el-row>

      <el-table :data="userList" border stripe>
        <el-table-column type="index" align="center"></el-table-column>
        <el-table-column prop="username" label="姓名" align="center"></el-table-column>
        <el-table-column prop="email" label="邮箱" align="center"></el-table-column>
        <el-table-column prop="mobile" label="手机号" align="center"></el-table-column>
        <el-table-column prop="role_name" label="角色名称" align="center"></el-table-column>
        <el-table-column prop="mg_state" label="状态" align="center">
          <template slot-scope="scope">
            <el-switch v-model="scope.row.mg_state" @change="changeUserState(scope.row)"></el-switch>
          </template>
        </el-table-column>
        <el-table-column label="操作" align="center" width="230px" fixed="right">
          <template slot-scope="scope">
            <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
            <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
            <el-tooltip effect="dark" content="权限设置" placement="top" :enterable="false">
              <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
            </el-tooltip>
          </template>
        </el-table-column>
      </el-table>

      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pagenum"
        :page-sizes="[1, 2, 5, 10]"
        :page-size="queryInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //获取用户对象参数
      queryInfo: {
        query: "", //查询参数
        pagenum: 1, //当前页码
        pagesize: 2, //每页显示条数
      },
      userList: [],
      total: 0,
    };
  },
  created() {
    this.getUserList();
  },
  methods: {
    getUserList() {
      this.$http.get("users", { params: this.queryInfo }).then((res) => {
        if (res.data.meta.status !== 200) {
          this.$message.error("获取用户列表失败");
          return;
        }
        this.userList = res.data.data.users;
        this.total = res.data.data.total;
        console.log("用户列表：", this.userList);
      });
	},
	handleSizeChange(newPage) {
		this.queryInfo.pagesize = newPage
		this.getUserList()
	},
	handleCurrentChange(cueePage) {
		this.queryInfo.pagenum = cueePage
		this.getUserList()
	},
	changeUserState(currData) {
		console.log(currData)
		this.$http.put(`users/${currData.id}/state/${currData.mg_state}`).then(res => {
			if(res.data.meta.status !== 200) {
				this.$message.error("更新用户状态失败！")
				return
			}
			this.$message.success("更新用户状态成功！")
		})
	}

  },
};
</script>
<style lang="less" scoped>
.users-ct {
  padding: 20px;
}
.el-row {
  margin-bottom: 20px;
}
.el-pagination{
	margin-top: 20px;
}
</style>