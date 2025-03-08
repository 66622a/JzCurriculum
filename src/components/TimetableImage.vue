<template>
  <div class="timetable-image-wrapper">
    <!-- 如果课表数据还未加载，则显示加载中 -->
    <div v-if="!timetableReady" class="loading">
      正在加载课表，请稍候...
    </div>
    <!-- 当课表数据加载完成后，再显示整个图片区域 -->
    <div v-else ref="timetableContainer" class="timetable-image" :style="{ backgroundImage: 'url(' + backgroundUrl + ')' }">
      <WeeklyTimetable :data="data" :selectedDate="selectedDate" />
      <div class="last-updated">Last Updated: {{ lastUpdated }}</div>
    </div>
    <!-- 只有在课表加载完成后才显示生成按钮 -->
    <button v-if="timetableReady" class="generate-btn" @click="generateImage">生成周课表图片</button>
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
      default: ''
    }
  },
  data() {
    return {
      lastUpdated: new Date().toLocaleString()
    }
  },
  computed: {
    // 当 data 数组不为空时，认为课表已加载完成
    timetableReady() {
      return this.data && this.data.length > 0
    }
  },
  methods: {
    generateImage() {
      html2canvas(this.$refs.timetableContainer).then(canvas => {
        const dataUrl = canvas.toDataURL('image/png')
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
  min-width: 800px;
  background-size: cover;
  background-position: center;
  padding: 20px;
}

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

.loading {
  font-size: 16px;
  text-align: center;
  padding: 40px;
}
</style>
