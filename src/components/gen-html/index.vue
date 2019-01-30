<template>
  <article>
    <header class="pd-l">
      <section>
        <el-steps :active="cur" finish-status="success">
          <el-step title="文本 1" description="将崇拜的步骤和信息按照示例格式输入到文本框"></el-step>
          <el-step title="解析 2" description="检查解析出的经文, 歌词, 流程是否有遗漏或错误"></el-step>
        </el-steps>
      </section>
    </header>
    <content class="lay-r">
      <input-vchurch v-if="cur==1"></input-vchurch>
      <html-preview :content="content" v-if="cur==2"></html-preview>
    </content>
    <footer>
      <li class="sd-cut ht-l pd-m-h">
        <button v-show='cur==2' class="sd-cut wd-xl" @click="previousStep">
          上一步
        </button>
        <button v-show='cur==1' class="sd-theme wd-xl" @click="nextStep">
          下一步
        </button>
      </li>
    </footer>
  </article>
</template>
<style type="text/css" scoped>
content {
  padding: 0 !important;
}

</style>
<script>
import inputVchurch from '../gen-ppt/input' 
import preview from './preview' 
export default {
  data(){
    return{
      cur: 1,
      content:''
    }
  },
  methods: {
    previousStep(){
      this.cur = 1
    },
    nextStep(){
      this.generate()
    },
    generate() {
      var inp = this.$store.state.inp
      if (!inp) return false
      var loading = this.$loading({
        fullscreen: true
      })
      this.$http.post('/api/gen-html', {
        worship:inp
      }).then(res => {
        loading.close()
        if (res.body.code!= 0) {
          return this.$notify.error({
            title: res.body.msg,
            message: res.body.msg,
            duration: 0
          });
        }
        this.$notify.success({
          title: res.body.msg,
          message:'可以下载PPT啦'
        });
        this.cur = 2
        this.content = res.body.data.content
      })
    },
  },
  components:{
    "input-vchurch": inputVchurch,
    "html-preview": preview
  }
};

</script>
