<template>
    <article>
      <header class="sd-bright">
        <section>
          <li class="ht-l pd-m">
            <span class='fs-xl'>{{ $route.name }}</span>
          </li>
          <li class="pd-m ht-l panel-nav align-c cl-sec">
            <span class="ht-l input-panel-nav wd-l" :class="{ 'cl-theme' : cur === 'input-panel'}">
              <div class="ht-s">
                <i class="fa fa-file-text-o fs-xl"></i>
              </div>
              <div class="ht-xs fs-s">文本</div>
            </span>
            <span class="wd-s">
              <i class="fa fa-angle-right fs-xl"></i>
            </span>
            <span class="ht-l parsed-panel-nav wd-l" :class="{ 'cl-theme' : cur === 'parsed-panel'}">
              <div class="ht-s">
                <i class="fa fa-list fs-xl"></i>
              </div>
              <div class="ht-xs fs-s">解析</div>
            </span>
            <span class="wd-s">
              <i class="fa fa-angle-right fs-xl"></i>
            </span>
            <span class="ht-l output-panel-nav wd-l" :class="{ 'cl-theme' : cur === 'output-panel'}">
              <div class="ht-s">
                <i class="fa fa-download fs-xl"></i>
              </div>
              <div class="ht-xs fs-s">导出</div>
            </span>
          </li>
        </section>
      </header>
      <content>
        <router-view></router-view>
      </content>
    </article>
</template>
<script>
export default {
  computed: {
    cur: function() {
      return this.$store.state.cur;
    }
  }
};
</script>
