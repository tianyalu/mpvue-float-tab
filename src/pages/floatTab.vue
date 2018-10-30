<template>
  <div class="container">
    <div class="occupy-div">
      固定的占位布局
    </div>

    <!-- 标签列表 -->
    <div :class="{'tag-fixed': !intersection && listDisplay}" class="normal">
      <tabTags :tags="tagList" :toView="tagName" :selectIndex="tagSelectIndex" :tagClick="selectTag"></tabTags>
    </div>
    <div style="height: 38px;" v-if="!intersection && listDisplay"></div>

    <!-- first-child属性必须在最外层加一层div，不知是何原因 -->
    <div class="list">
      <div v-for="(item, index) in dataList" :key="item.id" class="list-container">
        <div class="child-container">
          <div class="index">{{index + 1}}.</div>
          <div>{{item.name}}</div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
// import wx from 'wx'
import tabTags from '@/components/tags'
export default {
  data() {
    return {
      intersection: true,
      listDisplay: false,
      dataList: [],
      tagList: [],
      // 标签选中的索引
      tagSelectID: '',
      tagSelectIndex: 0,
      tagName: '',
      //列表距离顶部的距离
      listTop: 0
    }
  },
  onLoad() {
    wx
      .createIntersectionObserver()
      .relativeToViewport()
      .observe('.occupy-div', res => {
        // 控件本身与显示区域相交区域占目标节点的布局区域的比例
        this.intersection = res.intersectionRatio !== 0
      })
    wx
      .createIntersectionObserver()
      .relativeToViewport()
      .observe('.list', res => {
        // 控件本身与显示区域相交区域占目标节点的布局区域的比例
        this.listDisplay = res.intersectionRatio !== 0
      })
    this.initDataList()
    this.initTagList()
  },
  methods: {
    initDataList() {
      for (var i = 0; i < 20; i++) {
        this.dataList.push({ name: 'text item' })
      }
    },
    initTagList() {
      for (var i = 0; i < 8; i++) {
        this.tagList.push({ id: i, tag_name: '标签' + (i + 1) })
      }
    },
    selectTag(index = 0) {
      this.tagSelectIndex = index
      if (index < this.tagList.length) {
        this.tagSelectID = this.tagList[index].id
        if (this.tagSelectIndex < index) {
          this.tagName = 'A' + this.tagList[index].id
        } else {
          if (index >= 2) {
            this.tagName = 'A' + this.tagList[index - 2].id
          } else {
            this.tagName = 'A' + this.tagList[0].id
          }
        }
      }

      // 获取要滚动的控件的高度
      // var query = wx.createSelectorQuery()
      // query
      //   .select('.occupy-div')
      //   .boundingClientRect(rect => {
      //     this.listTop = rect.height
      //   })
      //   .exec()
      // 将列表滚动一定距离，以使标签处理屏幕顶端--此操作应对与下方tab所对应列表长度不一的情况
      // if (!this.intersection && this.listDisplay) {
      //   setTimeout(() => {
      //     if (this.listTop !== 0) {
      //       wx.pageScrollTo({
      //         scrollTop: this.listTop,
      //         duration: 0
      //       })
      //     }
      //   }, 100)
      // }

      wx.showToast({
        title: index + ' is clicked',
        icon: 'none',
        duration: 1000
      })
    }
  },
  components: {
    tabTags
  }
}
</script>


<style lang="scss" scoped>
.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  background: #f5f5f5;
}
.occupy-div {
  width: 100%;
  height: 400pt;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: #f5f5f5;
}

.normal{
  width: 100%;
  padding: 4pt 0;
  height: 38px;
  background: #f5f5f5;
}

.normal.tag-fixed {
  position: fixed;
  top: 0px;
  z-index: 999;
  width: 100%;
  padding: 4pt 0;
  height: 38px;
  background: #f5f5f5;
}

.list {
  width: 100%;
  background: white;
}
.list-container {
  display: flex;
  flex-direction: column;
}
.child-container {
  display: flex;
  align-items: center;
  padding: 6pt 16pt;
}
.list-container:first-child {
  margin-top: 12pt;
}
.list-container:last-child {
  padding-bottom: 12pt;
}
.index {
  margin-right: 8pt;
}
</style>

