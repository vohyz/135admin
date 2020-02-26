<template>
  <div>
    <el-form :model="newform">
      <el-form-item label="标题" :label-width="formLabelWidth">
        <el-input v-model="newform.caseTitle" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="国家" :label-width="formLabelWidth">
        <el-input v-model="newform.country" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="面积" :label-width="formLabelWidth">
        <el-input v-model="newform.areaNum" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="人口" :label-width="formLabelWidth">
        <el-input v-model="newform.population" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="地理位置" :label-width="formLabelWidth">
        <el-input v-model="newform.district" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="封面图" :label-width="formLabelWidth">
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
      <el-form-item label="内容图" :label-width="formLabelWidth">
        <el-upload
          action="#"
          ref="zhengwen"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview2"
          :auto-upload="false"
          :on-change="fileChange2"
          :limit="count+1">
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
          v-model="newform.content">
        </el-input>
      </el-form-item>
      <el-form-item label="来源" :label-width="formLabelWidth">
        <el-input
          v-model="newform.contentSource">
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
    name: 'newCase',
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
        dialogImageUrl1: '',
        dialogVisible1: false,
        dialogImageUrl2: '',
        dialogVisible2: false,
        formLabelWidth: '120px',
        count: 1,
        newform: {
          imgUrls: [],
          country: '',
          contentSource: '',
          district: '',
          areaNum: '',
          caseTitle: '',
          content: '',
          population: ''
        },
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
        this.newform.coverUrl = file.raw;
      },
      fileChange2(file) {
        this.newform.imgUrls.push(file.raw);
      },
      upload () {
        let form = new FormData();
        form.append('coverImg',this.newform.coverUrl);
        form.append('imgTwo',this.newform.imgUrls[1]);
        form.append('imgOne',this.newform.imgUrls[0]);
        form.append('country',this.newform.country);
        form.append('content', this.newform.content);
        form.append('district', this.newform.district);
        form.append('caseTitle', this.newform.caseTitle);
        form.append('areaNum', this.newform.areaNum);
        form.append('contentSource', this.newform.contentSource);
        form.append('population', this.newform.population);
        console.log(form.get('imgOne'))
        console.log(form.get('imgTwo'))
        this.$axios.post('/api/case/insertCase',form,{headers: {'Content-Type': 'multipart/form-data;boundary=-----------------------------264141203718551'}})
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
