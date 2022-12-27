<template>
  <div class="echarts-box" style="overflow: hidden">
    <div
      id="experiment"
      :style="{ width: width + 'px', height: '280px', paddingTop: '10px' }"
    ></div>
    <div>ddd:{{ message }}</div>
  </div>
</template>
  
<script setup>
import * as echarts from 'echarts'
import { onMounted, onUnmounted, ref } from 'vue'
import axios from 'axios'

/// 声明定义一下echart
let echart = echarts
let sopNum = ref()
let taskNum = ref()
let chart = null
let experimentSopList = ref([])
let message = ref('yyy')
let width = ref()
let height = ref()
let grapthData = ref([])

onMounted(() => {
  // 设置数据// 获取数据
  initChart()

  window.addEventListener('message', handleMessage)

  // 子组件传父组件
  window.parent.postMessage(
    {
      cmd: 'returnInfo',
      params: {
        info: '爸~隔壁王阿姨找你过去给她修电视',
      },
    },
    '*'
  )
})

const handleMessage = (event) => {
  var data = event.data
  console.log('data', data)
  var params = JSON.parse(data)
  width.value = params.width
  height.value = params.height
  grapthData.value = params.data
  initChart()
}

onUnmounted(() => {
  // echart.dispose
  if (!chart) {
    return
  }
  chart.dispose()
  chart = null
})

// 基础配置一下Echarts
const initChart = () => {
  chart = echart.init(document.getElementById('experiment'), 'dark')

  // 把配置和数据放这里
  chart.setOption({
    backgroundColor: '',
    xAxis: {
      type: 'category',
      data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    },
    yAxis: {
      type: 'value',
    },
    series: [
      {
        data: grapthData.value,
        type: 'bar',
      },
    ],
    grid: {
      x: 50,
      y: 20,
      x2: 70,
      y2: 30,
    },
  })

  window.onresize = function () {
    //自适应大小
    chart.resize()
  }
}
</script>

<style scoped>
.echarts-box::-webkit-scrollbar {
  width: 30px;
}

::-webkit-scrollbar {
  width: 30px;
  height: 5px;
}
</style>