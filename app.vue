<template>
  <!-- 可视区域 -->
  <div class="view-port" ref="viewPort" :style="{ '--rowHeight': rowHeight + 'px' }" @scroll="onScroll">
      <!-- 占位 -->
    <div class="scroll-bar" ref="scrollBar"> </div>
    <!-- 列表 -->
    <div class="list" ref="listRef">
      <div class="row" v-for="(item, index) in showList">
        {{ item.n }}
      </div>
    </div>

  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from "vue"
// 造20万数据，fill是填充的功能
const bigList = new Array(200000).fill(null).map((ele, i) => ({ n: i + 1 }))

//  冻结对象，Object.freeze vue不监听它的改变
let list = ref(Object.freeze(bigList))

// 每页显示数量
let viewCount = ref(20)
// 每行高度
let rowHeight = ref(20)
// 占位 ref
let scrollBar = ref()
// 可视窗口 ref
let viewPort = ref()
// 列表 ref
let listRef = ref()

onMounted(() => {
  console.log("挂载完成")
  //   滚动区域的高
  viewPort.value.style.height = (rowHeight.value * viewCount.value) + 'px'
  //  占位的高
  scrollBar.value.style.height = (rowHeight.value * list.value.length) + 'px'

})

//从bigList取数据 开始偏移量
let start = ref(0)
//从bigList取数据 结束偏移量
let end = ref(20)
//  显示的数据列表
const showList = computed(() => {
  return list.value.slice(start.value, end.value)
})

console.log("大数据 ", bigList);
console.log("showList ", showList.value)


const onScroll = () => {
  //  滚动偏移量
  let offsetTop = viewPort.value.scrollTop
  console.log("offsetTop " , offsetTop)
  // 滚动后，计算开始和结束位置
  start.value = Math.round(offsetTop / rowHeight.value)
  end.value = start.value + viewCount.value
  console.log(" start.value ",start.value ," end.value ",end.value )
  // list 要下移的位置 transform  或 paddingTop 都能实现效果
  // listRef.value.style.transform = `translateY(${offsetTop}px)`
    listRef.value.style.paddingTop = `${offsetTop}px`
}




</script>

<style lang="css" scoped>
.view-port {
  width: 300px;
  /* height: 300px; */

  background-color: yellowgreen;
  position: relative;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  overflow-y: scroll;
  overflow-x: hidden;
}

.list {
  width: 300px;
  position: absolute;
  left: 0;
  top: 0;
}

.row {
  /*height: 20px;*/
  height: var(--rowHeight);
}
</style>