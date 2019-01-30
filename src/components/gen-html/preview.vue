<template>
  <section class="content wd-max pd-s">
      <div class="wd-max ht-max sd-cut pd-s" style="line-height: 1.5em" v-html="content"></div>
  </section>
</template>
<style type="text/css" scoped>
  .content{
    position: absolute;
    top:0;
    bottom:0;
  }
</style>
<script>
export default {
  props:['content'],
  name: 'preview',
  data(){
    return{
      output:'',
    }
  },
}

</script>
