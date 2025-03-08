<template>
  <div>
    <!-- WeeklyTimetable组件 -->
    <WeeklyTimetable
      v-if="timetableData.length"
      :data="timetableData"
      :selectedDate="selectedDate"
      @timetable-loaded="onTimetableLoaded"
    />

    <!-- 生成图片区域，带背景 -->
    <div v-if="timetableLoaded" class="timetable-image-wrapper">
      <div ref="timetableContainer" class="timetable-image" :style="backgroundStyle">
        <WeeklyTimetable :data="timetableData" :selectedDate="selectedDate" />
        <div class="last-updated">
          Last Updated: {{ lastUpdated }}
        </div>
      </div>
      <button @click="generateImage">生成周课表图片</button>
      <img v-if="timetableImage" :src="timetableImage" alt="课表图片预览">
    </div>
  </div>
</template>

<script>
import WeeklyTimetable from './WeeklyTimetable.vue'
import html2canvas from 'html2canvas'

export default {
  components: { WeeklyTimetable },
  data() {
    return {
      timetableData: [],           // 从API加载的数据
      selectedDate: '2025-03-02',  // 当前选中的日期
      timetableLoaded: false,
      timetableImage: null,
      backgroundUrl: 'https://s2.loli.net/2025/03/08/VlesoEwDrIfJdYX.jpg' // 由你提供的背景图片URL
    }
  },
  computed: {
    backgroundStyle() {
      return {
        backgroundImage: `url(${this.backgroundUrl})`,
        backgroundSize: 'cover',
        backgroundPosition: 'center',
        padding: '20px'
      }
    }
  },
  methods: {
    onTimetableLoaded() {
      this.timetableLoaded = true
    },
    generateTimetableImage() {
      const element = this.$refs.timetableContainer;
      html2canvas(element, { useCORS: true }).then(canvas => {
        this.timetableImage = canvas.toDataURL('image/png')
        // 自动下载图片
        const link = document.createElement('a')
        link.href = this.timetableImage
        link.download = 'weekly_timetable.png'
        link.click()
      });
    }
  }
}
</script>

<style scoped>
.timetable-image-wrapper {
  margin-top: 20px;
  position: relative;
  width: 100%;
  overflow-x: auto;
}

.timetable-image {
  position: relative;
  min-width: 800px;
}

.last-updated {
  position: absolute;
  bottom: 10px;
  right: 10px;
  padding: 5px 10px;
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 4px;
  font-size: 12px;
}

.timetable-image-wrapper img {
  max-width: 100%;
  margin-top: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 6px 12px;
  background-color: #409eff;
  color: white;
  border-radius: 4px;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #66b1ff;
}
</style>
