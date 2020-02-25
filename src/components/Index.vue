<template>
<div class="bigbox">
  <el-row class="tac">
   <el-col>
     <el-menu
       default-active="2"
       class="el-menu-vertical-demo"
       @select="handleSelect">
          <el-menu-item index="1">
            <i class="el-icon-menu"></i>
            <span slot="title">新闻管理</span>
          </el-menu-item>
          <el-menu-item index="2">
            <i class="el-icon-menu"></i>
            <span slot="title">案例管理</span>
          </el-menu-item>
          <el-menu-item index="3">
            <i class="el-icon-document"></i>
            <span slot="title">管理员账号管理</span>
          </el-menu-item>
          <el-menu-item @click="logout()" >
            <i class="el-icon-setting"></i>
            <span slot="title">登出</span>
          </el-menu-item>
     </el-menu>
   </el-col>
 </el-row>
  <div class="content">
    <div v-if="Flag === '1'">
      <news></news>
    </div>
    <div v-if="Flag === '2'">
       <ccase></ccase>
    </div>
    <div v-if="Flag === '3'">
      <admin></admin>
    </div>
  </div>
</div>
</template>

<script>
  import ccase from './case.vue'
  import news from './news.vue'
  import admin from './admin.vue'
export default {
  name: 'Index',
  components: {
    news,
    ccase,
    admin
  },
  methods: {
    logout () {
      this.$store.dispatch('userLogout')
      this.$message({
        showClose: true,
        message: '登出成功',
        type: 'success'
      })
      localStorage.removeItem('User')
      localStorage.removeItem('UserName')
      localStorage.removeItem('userType')
      localStorage.removeItem('userId')
      localStorage.removeItem('User_avatar')
    },
    handleSelect(key, keyPath) {
      this.Flag = key
    },
    chooseModule (moduleName) {
      this.$axios.post('/api/task/findTaskByTags',
        {
          tags: moduleName
        }).then((response) => {
        if (response.data.Status === 'right') {
          this.taskList = response.data.task_omitinfo
        } else {
          alert(response.data.Details)
        }
      })
        .catch((error) => {
          console.log(error)
        })
    },
    gotoDetails (id) {
      let type = 'published'
      this.$axios.post('/addHistory', {
        'task_id': id,
        'user_name': localStorage.getItem('UserName')
      }).then((response) => {
        console.log(response.data)
      })
        .catch((error) => {
          console.log(error)
        })
      this.$router.push({
        path: `/taskdetails/${type}/${id}`
      })
    },
    Init () {
      this.$axios.post('/api/task/latest').then((response) => {
        this.taskList = response.data.latest
      })
        .catch((error) => {
          console.log(error)
        })
    }
  },
  data () {
    return {
      Flag: '0'
    }
  }
}
</script>

<style scoped>
  .bigbox {
    width: 100%;
    padding: 50px;
  }
  .tac {
    float: left;
    width: 300px;
  }
  .tac span {
    font-size:20px;
  }
  .content {
    float: left;
    width: 800px;
  }
</style>
