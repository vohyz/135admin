<style scoped>
  h2 {
    text-align: center;
    padding: 30px;
    font-size: 18px;
  }

  #chart_example {
    width: 800px;
    height: 500px;
    margin: 0 auto;
  }
  .box-card {
    padding-bottom:10px;
  }
</style>
<template>
  <div>
    <el-dialog title="添加管理员" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input v-model="form.account" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
        <el-input v-model="form.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="真实姓名" :label-width="formLabelWidth">
        <el-input v-model="form.real_name" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="upload()">确 定</el-button>
      </div>
    </el-dialog>
    <el-dialog title="注销" :visible.sync="dialogFormVisible1">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input v-model="form.account" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
        <el-input v-model="form.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="真实姓名" :label-width="formLabelWidth">
        <el-input v-model="form.real_name" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible1 = false">取 消</el-button>
        <el-button type="primary" @click="delete1()">确 定</el-button>
      </div>
    </el-dialog>
    <div style="width:100%;height:40px;">
      <el-button style="float:center" type="success" @click="dialogFormVisible = true">
        添加管理员
        </el-button>
      </div>
    <div style="width:100%;height:40px;margin-top:50px;">
      <el-button style="float:center" type="warning" @click="zhuxiao()">
        注销管理员(现在登录的管理员)
      </el-button>
    </div>
  </div>
</template>

<script>
  export default {
    name:'admin',
    data() {
      return {
        count: 1,
        dialogTableVisible: false,
        dialogFormVisible: false,
        dialogFormVisible1 : false,
        form: {
          account: '',
          password: '',
          real_name: ''
        },
        formLabelWidth: '120px'
      }
    },
    methods: {
      zhuxiao () {
        this.dialogFormVisible1 = true
      },
      delete1 () {
        var id = this.form.account
        this.$axios.post(`/api/admin/deleteAdmin/${id}`)
        this.$parent.logout()
      },
      upload () {
        this.$axios.post('/api/admin/insertAdmin',
        {
          'account': this.form.account,
          'password': this.form.password,
          'real_name': this.form.real_name
          }
        )
          .then((response) => {
            if (response.data == '1') {
              this.$message.success({
                message: '上传成功',
                showClose: true,
                type: 'success'
              })
              this.dialogFormVisible = false
            } else {
              this.$message.error({
                message: '上传失败',
                showClose: true,
                type: 'error'
              })
            }
          },
          (response) => {
            this.$message.error({
              message: '上传失败',
              showClose: true,
              type: 'error'
            })
          }
          )
        }
    },
    mounted() {
    }
  }
</script>
