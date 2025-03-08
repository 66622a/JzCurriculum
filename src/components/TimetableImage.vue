<template>
    <div class="timetable-image-wrapper">
      <!-- 生成图片的区域 -->
      <div ref="timetableContainer" class="timetable-image" :style="{ backgroundImage: 'url(' + backgroundUrl + ')' }">
        <!-- 使用原来的周课表组件 -->
        <WeeklyTimetable :data="data" :selectedDate="selectedDate" />
        <!-- 显示更新时间 -->
        <div class="last-updated">Last Updated: {{ lastUpdated }}</div>
      </div>
      <!-- 生成图片按钮 -->
      <button class="generate-btn" @click="generateImage">生成周课表图片</button>
    </div>
  </template>
  
  <script>
  import WeeklyTimetable from './WeeklyTimetable.vue'
  import html2canvas from 'html2canvas'
  
  export default {
    name: 'TimetableImage',
    components: { WeeklyTimetable },
    props: {
      data: {
        type: Array,
        default: () => []
      },
      selectedDate: {
        type: String,
        required: true
      },
      // 背景图片 URL，由你提供
      backgroundUrl: {
        type: String,
        default: 'https://s2.loli.net/2025/03/08/VlesoEwDrIfJdYX.jpg'
      }
    },
    data() {
      return {
        lastUpdated: ''
      }
    },
    mounted() {
      // 设置最后更新时间
      this.lastUpdated = new Date().toLocaleString()
    },
    methods: {
      generateImage() {
        // 使用 html2canvas 将 timetableContainer 转换为 canvas
        html2canvas(this.$refs.timetableContainer).then(canvas => {
          // 获取图片的 data URL
          const dataUrl = canvas.toDataURL('image/png')
          // 创建一个临时链接用于下载
          const link = document.createElement('a')
          link.href = dataUrl
          link.download = 'weekly_timetable.png'
          link.click()
        })
      }
    }
  }
  </script>
  
  <style scoped>
  .timetable-image-wrapper {
    width: 100%;
    padding: 20px;
  }
  
  .timetable-image {
    position: relative;
    width: 100%;
    /* 使用你需要的最小宽度，保证在电脑端能按照 WeeklyTimetable 的样式显示 */
    min-width: 800px;
    background-size: cover;
    background-position: center;
    padding: 20px;
  }
  
  /* 最后更新时间样式 */
  .last-updated {
    position: absolute;
    bottom: 10px;
    right: 10px;
    font-size: 14px;
    color: #333;
    background-color: rgba(255, 255, 255, 0.7);
    padding: 5px 10px;
    border-radius: 4px;
  }
  
  /* 按钮样式 */
  .generate-btn {
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #409eff;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .generate-btn:hover {
    background-color: #66b1ff;
  }
  </style>
  