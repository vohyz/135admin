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
    <el-dialog title="新建案例" :visible.sync="dialogFormVisible">
      <newCase :flash="flash" :close="close"></newCase>
    </el-dialog>
    <el-dialog title="案例详情" :visible.sync="dialogFormVisible2">
      <caseDetails :flash="flash" :close="close" :newform="changeform"></caseDetails>
    </el-dialog>
    <div style="width:100%;height:40px;"><el-button style="float:left" type="success" @click="dialogFormVisible = true">新建案例</el-button></div>
    <div>
    <el-table
      :data="casedata.slice((currentPage-1)*pagesize,currentPage*pagesize)"
      style="width: 100%">
      <el-table-column
        label="编号"
        width="60">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.caseId }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="封面"
        width="120">
        <template slot-scope="scope">
          <img :src="backurl+scope.row.coverUrl"/>
        </template>
      </el-table-column>
      <el-table-column
        label="标题"
        width="400">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.caseTitle }}</span>
        </template>
      </el-table-column>


      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="changeNews(scope.row)">详情</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="deleteNews(scope.row.caseId)" plain>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    </div>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[5, 6, 7, 8]"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="casedata.length">
    </el-pagination>
  </div>
</template>

<script>
  import qs from 'qs'
  import newCase from './newCase.vue'
  import caseDetails from './caseDetails.vue'
  export default {
    components: {
      newCase,
      caseDetails
    },
    data() {
      return {
        casedata: [],
        currentPage:1,
        pagesize:5,
        backurl: 'http://127.0.0.1:7878/',
        count: 1,
        startFrom: 0,
        limitation: 100,
        dialogFormVisible: false,
        dialogFormVisible2: false,
        changeform: {},
        formLabelWidth: '120px'
      }
    },
    methods: {
      handleSizeChange (size) {
        this.pagesize = size;
      },
      handleCurrentChange(currentPage){
        this.currentPage = currentPage;
      },
      handlePictureCardPreview1(file) {
        this.dialogImageUrl1 = file.url;
        this.dialogVisible1 = true;
      },
      handlePictureCardPreview2(file) {
        this.dialogImageUrl2 = file.url;
        this.dialogVisible2 = true;
      },
      fileChange1(file) {
        this.newform.file1 = file.raw;
      },
      fileChange2(file) {
        this.newform.file2 = file.raw;
      },
      deleteNews (id) {
        var id = id
        this.$axios.post(`/api/case/deleteCase/${id}`)
        this.$message.success({
          message: '删除成功',
          showClose: true,
          type: 'success'
        })
        this.flash()
      },
      changeNews (form) {
        this.$axios.post(`/api/case/queryCaseById/${form.caseId}`)
        .then((response) => {
          this.changeform = response.data
          console.log(response.data)
          this.dialogFormVisible2 = true
        })
      },
      flash () {
        this.$axios.post('/api/case/caseList',
        {
            'startFrom': this.startFrom,
            'limitation': this.limitation
        })
          .then((response) => {
            this.casedata = response.data
            
          },
          (response) => {
            this.$message.error({
              message: '获取失败',
              showClose: true,
              type: 'error'
            })
          }
          )
      },
      close () {
        this.dialogFormVisible = false
        this.dialogFormVisible2 = false
      }
    },
    mounted() {
      this.flash()
    }
  }
</script>
