<template>
  <div>
    <el-form :model="newform">
      <el-form-item label="标题" :label-width="formLabelWidth">
        <div>{{newform.title}}</div>
      </el-form-item>
      <el-form-item label="上传时间" :label-width="formLabelWidth">
        <div>{{newform.updateTime}}</div>
      </el-form-item>
      <el-form-item label="标签" :label-width="formLabelWidth">
        <div>{{newform.genre}}</div>
      </el-form-item>
      <el-form-item label="摘要" :label-width="formLabelWidth">
        <div>{{newform.introduction}}</div>
      </el-form-item>
      <el-form-item label="封面图" :label-width="formLabelWidth">
        <el-image
              style="width: 100px; height: 100px"
              :src="backurl+newform.coverUrl"></el-image>
      </el-form-item>
      <el-form-item label="正文图" :label-width="formLabelWidth">
        <el-image
              style="width: 100px; height: 100px"
              :src="backurl+newform.imgUrl"></el-image>
      </el-form-item>
      <el-form-item label="正文" :label-width="formLabelWidth">
        <div>{{newform.content}}</div>
      </el-form-item>
      <el-form-item label="来源/作者" :label-width="formLabelWidth">
        <div>{{newform.newsSource}}</div>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="close()">关 闭</el-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'changeNews',
    props: {
      close: {
        type: Function,
        default: null
      },
      flash: {
        type: Function,
        default: null
      },
      newform: {}
    },
    data() {
      return {
        backurl: 'http://127.0.0.1:7878/',
      };
    },
    methods: {
    }
  }
</script>
