<template>
  <section class="pd-m">
    <dl>
      <li class="bg-bright pd-m-h ht-l">
        <span>文件已经导出</span>
      </li>
      <div class="output-info pd-m pointer" @click='download'>{{ distPath }}</div>
    </dl>
    <dl class="mg-m-v">
      <li class="bg-bright pd-m-h ht-l">
        <span>公众号的文章</span>
        <div></div>
          <button class="sd-theme wd-xl" @click="copy">点击复制</button>
      </li>
      <div class="output-info pd-m pointer" ref='content'>
        <p :style="thStyle">信息主题：</p>
        <p :style='pStyle'>{{ worship.pTitle }}</p>
        <p :style="thStyle">信息经文：</p>
        <p :style='pStyle'>{{ worship.pScripture }}</p>
        <p :style='thStyle'>宣&nbsp;&nbsp;&nbsp;&nbsp;召：</p>
        <p :style="pStyle">{{ worship.call }}</p>
        <p :style='thStyle'>启应经文：</p>
        <p :style='pStyle'>{{worship.lection}}</p>
        <p :style='thStyle'>诗班献诗：</p>
        <p :style='pStyle'>{{ worship.choir }}</p>
        <p :style='thStyle'>诗歌：</p>
        <p v-for='hymn in hymnList' :style='pStyle'>
          <span>新编赞美诗歌：{{hymn.sn}}</span>
          <span>&nbsp;{{hymn.title}}&nbsp;</span>
        </p>
        <p :style='thStyle'>时间：</p>
        <p :style='pStyle'>{{ worship.time }}</p>
        <p :style='thStyle'>地点：</p>
        <p :style='pStyle'>{{ worship.place }}</p>
        <p :style='thStyle'>附近交通信息：</p>
        <p :style='pStyle'>深南大道9966号</p>
        <p :style='pStyle'>地铁：①号罗宝线-深大站 A3出口直行80-100米</p>
        <p :style='pStyle'>公交：深大北门①</p>
      </div>
    </dl>
  </section>
</template>
<script>
export default {
  mounted() {
    if (!this.distPath) {
      this.$store.commit('setCur', 'parsed')
      this.$router.push("/gen-ppt/parsed")
    }
  },
  data() {
    return {
      thStyle: {
        'max-width': '100%',
        'color': '#9e9e9e',
        'margin-top': '16px',
        'font-size': '14px',
        'line-height': '24px',
        'font-weight': '800',
        'box-sizing': 'border-box!important',
        'word-wrap': 'break-word!important'
      },
      pStyle: {
        'max-width': '100%',
        'color': '#424242',
        'font-size': '16px',
        'line-height': '1.4em',
        'box-sizing': 'border-box!important',
        'word-wrap': 'break-word!important'
      }
    }
  },
  methods: {
    download: function() {
      window.location.href = this.distPath
    },
    copy(){
      var txt = this.$refs.content.innerHTML
      if (!document.execCommand){
        this.$notify.info({
          title: '复制文本',
          message:'浏览器不支持复制请使用Chrome'
        });
      }
      var _id = new Date().valueOf();
      var textarea = document.createElement('textarea')
      textarea.style = 'opacity:0; position: absolute; z-index: -1;'
      textarea.id = _id
      textarea.innerHTML = txt
      document.body.append(textarea)
      var ta = document.getElementById(_id)
      ta.select()
      document.execCommand('copy')
      ta.parentNode.removeChild(ta)
      this.$notify.success({
        title: '复制文本',
        message:'已经复制文章到剪切板'
      });
    }
  },
  computed: {
    distPath: function() {
      return this.$store.state.distPath
    },
    hymnList() {
      if (!this.$store.state.worship) {
        return null
      }
      var hymnList = {
        pHymn: '颂赞诗歌',
        hCommunicationHymn: '圣餐诗歌',
        rHymn: '回应诗歌',
        song: '三一颂',
      }
      var res = []
      this.$store.state.worship.forEach(item => {
        if (hymnList[item.key]) {
          res.push(item)
        }
      })
      return res
    },
    worship: function() {
      if (!this.$store.state.worship) {
        return null
      }
      var list = {
        name: '威盛职场团契周六信息分享',
        call: '宣召',
        lection: '启应经文',
        hCommunication: '圣餐',
        hCommunicationScripture: '圣餐经文',
        choir: '诗班献诗',
        pScripture: '信息经文',
        pTitle: '信息主题',
        time: '时间',
        place: '地点',
      }
      this.$store.state.worship.forEach(item => {
        if (list[item.key]) {
          list[item.key] = item.value
        }
      })
      return list
    }
  }
}

</script>
