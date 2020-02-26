<template>
  <div>
    <el-form :model="newform">
      <el-form-item label="标题" :label-width="formLabelWidth">
        <el-input v-model="newform.title" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="上传时间" :label-width="formLabelWidth">
      <el-date-picker
        v-model="newform.time"
        type="date"
        value-format="yyyy-MM-dd"
        placeholder="选择日期">
      </el-date-picker>
      </el-form-item>
      <el-form-item label="标签" :label-width="formLabelWidth">
        <el-radio-group v-model="newform.radio">
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
          v-model="newform.zhaiyao">
        </el-input>
      </el-form-item>
      <el-form-item label="封面图" :label-width="formLabelWidth">
        <el-input v-model="newform.fengmianming" placeholder="封面图名字" style="width:130px;float:left">
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
        <el-input v-model="newform.zhengwenming" placeholder="正文图名字" style="width:130px;float:left">
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
          v-model="newform.zhengwen">
        </el-input>
      </el-form-item>
      <el-form-item label="来源/作者" :label-width="formLabelWidth">
        <el-input
          v-model="newform.resource">
        </el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="close()">取 消</el-button>
      <el-button type="primary" @click="upload()">确 定</el-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'newNews',
    props: {
      close: {
        type: Function,
        default: null
      },
      flash: {
        type: Function,
        default: null
      }
    },
    data() {
      return {
        label: ['项目新闻', '新区会议新闻','新区技术新闻'],
        dialogImageUrl1: '',
        dialogVisible1: false,
        dialogImageUrl2: '',
        dialogVisible2: false,
        formLabelWidth: '120px',
        count: 1,
        newform: {
          title: '',
          time: '',
          radio: '项目新闻',
          zhaiyao: '',
          fengmianming: '',
          zhengwenming: '',
          zhengwen: '',
          resource: ''
        },
      };
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
        this.newform.file1 = file.raw;
      },
      fileChange2(file) {
        this.newform.file2 = file.raw;
      },
      upload () {
        let form = new FormData();
        form.append('coverName',this.newform.fengmianming);
        form.append('imgName',this.newform.zhengwenming);
        form.append('cover',this.newform.file1);
        form.append('img',this.newform.file2);
        form.append('content', this.newform.zhengwen);
        form.append('genre', this.label[this.newform.radio-1]);
        form.append('introduction', this.newform.zhaiyao);
        form.append('link', '...');
        form.append('newsSource', this.newform.resource);
        form.append('title', this.newform.title);
        form.append('updateTime', this.newform.time);
        this.$axios.post('/api/news/insertNews',form,{headers: {'Content-Type': 'multipart/form-data;boundary=-----------------------------264141203718551'}})
          .then((response) => {
            if (response.data == '1') {
              this.$message.success({
                message: '上传成功',
                showClose: true,
                type: 'success'
              })
              this.close()
              this.flash()
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
    }
  }
</script>
