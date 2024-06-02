<template>

  <div class="view-port" ref="viewPort" :style="{ '--rowHeight': rowHeight + 'px' }" @scroll="onScroll">
    <div class="scroll-bar" ref="scrollBar"> </div>
    <div class="list" ref="listRef">
      <div class="row" v-for="(item, index) in showList">
        {{ item.n }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, computed } from "vue"

const bigList = new Array(200000).fill(null).map((ele, i) => ({ n: i + 1 }))


let list = ref(Object.freeze(bigList))
let start = ref(0)
let end = ref(20)
let viewCount = ref(20)
let rowHeight = ref(20)

let scrollBar = ref()
let viewPort = ref()
let listRef = ref()

onMounted(() => {
  console.log("挂载完成")
  viewPort.value.style.height = (rowHeight.value * viewCount.value) + 'px'
  scrollBar.value.style.height = (rowHeight.value * list.value.length) + 'px'

})

const showList = computed(() => {
  return list.value.slice(start.value, end.value)
})

console.log("大数据 ", bigList);
console.log("showList ", showList.value)


const onScroll = () => {
  let offsetTop = viewPort.value.scrollTop
  console.log("offsetTop " , offsetTop)
  start.value = Math.round(offsetTop / rowHeight.value)
  end.value = start.value + viewCount.value
  listRef.value.style.transform = `translateY(${offsetTop}px)`
 
}




</script>

<style lang="css" scoped>
.view-port {
  width: 300px;
  height: 300px;

  background-color: yellowgreen;
  position: relative;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto;
  overflow-y: scroll;
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