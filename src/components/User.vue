<template>
  <div>
    <!--    面包屑导航-->
    <el-breadcrumb style="padding-bottom:15px " separator="/">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>系统管理</el-breadcrumb-item>
      <el-breadcrumb-item>角色管理</el-breadcrumb-item>
    </el-breadcrumb>
    <!--    页面内容-->
    <el-card>
      <!--    搜索头部-->
      <el-row :gutter="40" style="padding-bottom: 15px">
        <el-col :span="8">
          <el-input placeholder="角色名" v-model="input3" class="input-with-select"
                    clearable>
          </el-input>
        </el-col>
        <el-button>搜索</el-button>
        <el-button>重置</el-button>
        <el-button>添加</el-button>
          <el-button type="danger" style="position: absolute;left: 90%">批量删除</el-button>
      </el-row>
      <!--      渲染数据表格-->
      <el-table
          ref="multipleTable"
          :data="tableData"
          tooltip-effect="dark"
          style="width: 100%"
          @selection-change="handleSelectionChange">
        <el-table-column
            type="selection"
            width="55">
        </el-table-column>
        <el-table-column
            prop="date"
            label="角色名称"
            width="360">
        </el-table-column>
        <el-table-column
            prop="name"
            label="角色描述"
            width="417">
        </el-table-column>
        <el-table-column
            prop="u_id"
            label="操作"
            width="420">
          <div slot-scope="scope">
            <el-button type="primary" size="mini" @click="updateUser(scope.row)">修改</el-button>
            <el-button type="primary" size="mini" >详情</el-button>
            <el-button type="danger" size="mini" >删除</el-button>
          </div>
        </el-table-column>
      </el-table>
      <!--      分页控件-->
      <el-pagination
          @size-change="SizeChange"
          @current-change="CurrentChange"
          :current-page="page"
          :page-sizes="[10, 20, 50, 100]"
          :page-size="limit"
          layout="total, sizes, prev, pager, next, jumper"
          :total="UserList.count">
      </el-pagination>
      <el-dialog
          title="修改角色"
          :visible.sync="dialogUpdateUser"
          width="30%">
        <!--        内容区域-->
        <el-form :model="UpdateUserList"  ref="ruleForm" label-width="100px" class="demo-ruleForm">
          <el-form-item label="角色名称" prop="date">
            <el-input v-model="UpdateUserList.date" ></el-input>
          </el-form-item>
          <el-form-item label="角色描述" prop="name">
            <el-input v-model="UpdateUserList.name"  type="textarea"></el-input>
          </el-form-item>
<!--          <el-form-item label="手机号" prop="phonenum">-->
<!--            <el-input v-model="UpdateUserList.phonenum"></el-input>-->
<!--          </el-form-item>-->
        </el-form>
        <span slot="footer" class="dialog-footer">
    <el-button @click="dialogUpdateUser = false">取 消</el-button>
    <el-button type="primary" >保 存</el-button>
  </span>
      </el-dialog>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "User",
  //数据
  data() {
    return {
      input3: null,
      page: 1,
      limit: 10,
      UserList: [],
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333
      }],
      UpdateUserList: {
        date: '',
        name: '',
      },
      dialogUpdateUser:false,
      //验证规则
      // rules: {
      //   address: [
      //     {required: true, validator: this.phoneRules, trigger: 'blur'}
      //   ],
      //   user_id: [{required: true, message: '请输入证件号', trigger: 'blur'},
      //     {
      //       pattern: /(^\d{8}(0\d|10|11|12)([0-2]\d|30|31)\d{3}$)|(^\d{6}(18|19|20)\d{2}(0\d|10|11|12)([0-2]\d|30|31)\d{3}(\d|X|x)$)/,
      //       message: '请输入正确的证件号'
      //     },
      //     {validator: this.IdentityCodeValid, trigger: 'blur'}],
      // }
    }
  },
  //方法
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    // //添加
    // addUser(){
    //   this.dialogAddUser = !this.dialogAddUser
    // },
    // addUser1(){
    //   this.dialogAddUser = !this.dialogAddUser,
    //       this.$http.get("/UserController/addUser", {
    //         params: {
    //           u_id: this.AddUserList.u_id,
    //           username: this.AddUserList.username,
    //           password: this.AddUserList.password,
    //           sex: this.AddUserList.sex,
    //           phonenum: this.AddUserList.phonenum,
    //         }
    //       }).then((res) => {
    //         console.log(res)
    //         this.UserList = res.data;
    //         if (res.data.code === 1) {
    //           this.$message.success("添加成功")
    //           this.getUserList();
    //         } else this.$message.error("添加失败")
    //       })
    // },
    // //关键字搜索
    // Search() {
    //   console.log(this.input3)
    //   this.$http.get("/UserController/selectUser", {
    //     params: {
    //       page: 1,
    //       limit: 10,
    //       Keyword: this.input3,
    //     }
    //   }).then((res) => {
    //     console.log(res.data)
    //     this.UserList = res.data;
    //     if (res.data.code === 1) {
    //       this.$message.success("搜索成功")
    //     } else this.$message.error("搜索失败")
    //   })
    // },
    //请求查看用户列表数据
    getUserList() {
      // this.$http.get("", {
      //   params: {
      //     page: this.page,
      //     limit: this.limit,
      //   }
      // }).then((res) => {
      //   // console.log(res)
      //   this.UserList = res.data;
      //   if (res.data.code === 1) {
      //     // this.$message.success("请求成功")
      //   } else this.$message.error("请求失败")
      // })
    },
    // //编辑按钮事件
    updateUser(row) {
      this.dialogUpdateUser = !this.dialogUpdateUser
      this.UpdateUserList = row;
      console.log(this.UpdateUserList)
    },
    // //确定编辑事件
    // updateUser1() {
    //   this.dialogUpdateUser = !this.dialogUpdateUser,
    //       this.$http.get("/UserController/updataUser", {
    //         params: {
    //           user: this.UpdateUserList,
    //           u_id: this.UpdateUserList.u_id,
    //           username: this.UpdateUserList.username,
    //           password: this.UpdateUserList.password,
    //           sex: this.UpdateUserList.sex,
    //           phonenum: this.UpdateUserList.phonenum,
    //         }
    //       }).then((res) => {
    //         console.log(res)
    //         this.UserList = res.data;
    //         if (res.data.code === 1) {
    //           this.$message.success("保存成功")
    //           this.getUserList();
    //         } else this.$message.error("保存失败")
    //       })
    // },
    // //删除按钮事件
    // deleteUser(row) {
    //   this.$confirm('此操作将永久删除该用户, 是否继续?', '提示', {
    //     confirmButtonText: '确定',
    //     cancelButtonText: '取消',
    //     type: 'warning'
    //   }).then(() => {
    //     this.$http.get("/UserController/deleteUser", {
    //       params: {
    //         u_id: row.u_id,
    //       }
    //     }).then((res) => {
    //       console.log(res)
    //       if (res.data.code === 1) {
    //         this.$message.success("删除成功")
    //         this.getUserList();
    //       } else this.$message.error("删除失败")
    //     })
    //   }).catch(() => {
    //     this.$message({
    //       type: 'info',
    //       message: '已取消删除'
    //     });
    //   });
    // },
    //每页条数发生改变时触发
    SizeChange(limit) {
      this.limit = limit;
      // this.getUserList();
    },
    //页码发生改变时触发
    CurrentChange(page) {
      this.page = page;
      // this.getUserList();
    },
  },
  //  钩子函数
  created() {
    // this.getUserList();
  }
}
</script>

<style scoped>
.a{
  margin-left: 360px;
  margin-top: 20px;
}
</style>
