<template>
  <div class="container">
    <scroll-view scroll-x="true" class="scroll-view" :scroll-into-view="toView" scroll-with-animation="true">
      <div class="tab-container" v-for="(item, index) in tags" :key="item.id">
        <div :class="{'selected': selectIndex === index}" @click="tagClick(index)" class="normal" :id="'A' + item.id" >
          {{item.tag_name}}
        </div>
        <div class="indicator" v-if="selectIndex === index"></div>
      </div>
    </scroll-view>
  </div> 
</template>
<script>
export default {
  data() {
    return {}
  },
  props: ['tags', 'selectIndex', 'tagClick', 'toView']
}
</script>

<style lang="scss" scoped>
.container {
  height: 38px;
  overflow: hidden;
  margin: 0 6pt;
  background: white;
}
.scroll-view {
  width: 100%;
  height: 46px;
  white-space: nowrap;
  overflow: scroll;
}
.tab-container {
  position: relative;
  display: inline-block;
  height: 36px;
  width: 20%;
}
.normal.selected {
  font-size: 12pt;
  font-weight: 600;
  color: #fd5a43;
}
.normal {
  height: 100%;
  flex: 1;
  font-size: 10pt;
  line-height: 36px;
  text-align: center;
}
.indicator {
  position: absolute;
  height: 2pt;
  width: 80%;
  margin: 0 10%;
  background: #fd5a43;
  bottom: 0;
}
</style>

