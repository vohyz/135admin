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
    <el-dialog title="新建新闻" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="标题" :label-width="formLabelWidth">
          <el-input v-model="form.title" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="上传时间" :label-width="formLabelWidth">
        <el-date-picker
          v-model="form.time"
          type="datetime"
          placeholder="选择日期时间">
        </el-date-picker>
        </el-form-item>
        <el-form-item label="标签" :label-width="formLabelWidth">
          <el-radio-group v-model="form.radio">
            <el-radio :label="1">项目新闻</el-radio>
            <el-radio :label="2">新区会议新闻</el-radio>
            <el-radio :label="3">新区技术新闻</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="摘要" :label-width="formLabelWidth">
          <el-input
            type="textarea"
            :autosize="{ minRows: 2, maxRows: 4}"
            placeholder="请输入摘要"
            v-model="form.zhaiyao">
          </el-input>
        </el-form-item>
        <el-form-item label="封面图" :label-width="formLabelWidth">
          <el-input v-model="form.fengmianming" placeholder="封面图名字" style="width:130px;float:left">
          </el-input>
          <el-upload
            action="#"
            ref="fengmian"
            list-type="picture-card"
            :on-preview="handlePictureCardPreview1"
            :auto-upload="false"
            :on-change="fileChange1"
            :limit="count">
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible1">
            <img width="100%" :src="dialogImageUrl1" alt="">
          </el-dialog>
        </el-form-item>
        <el-form-item label="正文图" :label-width="formLabelWidth">
          <el-input v-model="form.zhengwenming" placeholder="正文图名字" style="width:130px;float:left">
          </el-input>
          <el-upload
            action="#"
            ref="zhengwen"
            list-type="picture-card"
            :on-preview="handlePictureCardPreview2"
            :auto-upload="false"
            :on-change="fileChange2"
            :limit="count">
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible2">
            <img width="100%" :src="dialogImageUrl2" alt="">
          </el-dialog>
        </el-form-item>
        <el-form-item label="正文" :label-width="formLabelWidth">
          <el-input
            type="textarea"
            :autosize="{ minRows: 2, maxRows: 6}"
            placeholder="请输入正文"
            v-model="form.zhengwen">
          </el-input>
        </el-form-item>
        <el-form-item label="来源/作者" :label-width="formLabelWidth">
          <el-input
            v-model="form.resource">
          </el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="upload()">确 定</el-button>
      </div>
    </el-dialog>
    <div style="width:100%;height:40px;"><el-button style="float:left" type="success" @click="dialogFormVisible = true">添加新闻</el-button></div>
    <div>
    <el-card class="box-card" v-for="o in 4" :key="o">
     <div style="float:left"></div>
     <el-button style="float:right" type="danger" plain>删除</el-button>
     <el-button style="float:right" type="primary" plain>详情</el-button>
    </el-card>
    </div>
    <el-pagination
        layout="prev, pager, next"
        :total="10">
    </el-pagination>
  </div>
</template>

<script>
  import pic from './picUpload.vue'
  export default {
    components: {
      pic
    },
    data() {
      return {
        count: 1,
        dialogTableVisible: false,
        dialogFormVisible: false,
        dialogImageUrl1: '',
        dialogVisible1: false,
        dialogImageUrl2: '',
        dialogVisible2: false,
        form: {
          title: '',
          time: '',
          radio: 1,
          zhaiyao: '',
          fengmianming: '',
          zhengwenming: '',
          zhengwen: '',
          resource: ''
        },
        formLabelWidth: '120px'
      }
    },
    methods: {
      handlePictureCardPreview1(file) {
        this.dialogImageUrl1 = file.url;
        this.dialogVisible1 = true;
      },
      handlePictureCardPreview2(file) {
        this.dialogImageUrl2 = file.url;
        this.dialogVisible2 = true;
      },
      fileChange1(file) {
        this.file1 = file.raw;
      },
      fileChange2(file) {
        this.file2 = file.raw;
      },
      upload () {
        const formData = new FormData();
        formData.append('coverName',this.form.fengmianming)
        formData.append('imgName',this.form.zhengwenming)
        formData.append('cover',this.file1)
        formData.append('img',this.file2)
        formData.append('newsEdit',{
          'content': this.form.zhengwen,
          'genre': this.form.radio,
          'introduction': this.form.zhaiyao,
          'link': '...',
          'newsSource': this.form.resource,
          'titile': this.form.title,
          'updateTime': this.form.time
        })
        alert(this.form.fengmianming)
        this.$axios.post('/api/news/insertNews',
        {
          'coverName':this.form.fengmianming,
          'imgName':this.form.zhengwenming,
          'cover':this.file1,
          'img':this.file2,
          'newsEdit':{
            'content': this.form.zhengwen,
            'genre': this.form.radio,
            'introduction': this.form.zhaiyao,
            'link': '...',
            'newsSource': this.form.resource,
            'titile': this.form.title,
            'updateTime': this.form.time,
          }
        },{headers: {'Content-Type': 'multipart/form-data'}})
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
