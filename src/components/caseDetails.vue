<template>
  <div>
    <el-form :model="newform">
      <el-form-item label="标题" :label-width="formLabelWidth">
        <div>{{newform.caseTitle}}</div>
      </el-form-item>
      <el-form-item label="国家" :label-width="formLabelWidth">
        <div>{{newform.country}}</div>
      </el-form-item>
      <el-form-item label="面积" :label-width="formLabelWidth">
        <div>{{newform.areaNum}}</div>
      </el-form-item>
      <el-form-item label="人口" :label-width="formLabelWidth">
        <div>{{newform.population}}</div>
      </el-form-item>
      <el-form-item label="地理位置" :label-width="formLabelWidth">
        <div>{{newform.district}}</div>
      </el-form-item>
      <el-form-item label="封面图" :label-width="formLabelWidth">
        <el-image
              style="width: 100px; height: 100px"
              :src="backurl+newform.coverUrl"></el-image>
      </el-form-item>
      <el-form-item label="内容图" :label-width="formLabelWidth">
        <el-image
              style="width: 100px; height: 100px"
              :src="backurl+newform.imgUrls[0]"></el-image>
              <el-image
                    style="width: 100px; height: 100px"
                    :src="backurl+newform.imgUrls[1]"></el-image>
      </el-form-item>

      <el-form-item label="正文" :label-width="formLabelWidth">
        <div>{{newform.content}}
        </div>
      </el-form-item>
      <el-form-item label="来源" :label-width="formLabelWidth">
        <div>{{newform.contentSource}}
        </div>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="close()">关 闭</el-button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'caseDetails',
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
        formLabelWidth: '120px',
        backurl: 'http://127.0.0.1:7878/',
        count: 1
      };
    },
    methods: {
    }
  }
</script>
