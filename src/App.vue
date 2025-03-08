<template>
  <router-view />
  <TimetableImage 
      :data="timetableData" 
      :selectedDate="currentDate" 
      backgroundUrl="https://s2.loli.net/2025/03/08/VlesoEwDrIfJdYX.jpg" 
  />
</template>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>

<script>
import TimetableImage from './components/TimetableImage.vue'

export default {
  name: 'App',
  components: {
    TimetableImage
  },
  data() {
    return {
      timetableData: [],  // 传入 WeeklyTimetable 所需的课表数据
      currentDate: '2025-03-02'  // 当前选中的日期
    }
  },
  created() {
    // 根据需要加载或设置课表数据
  }
}
</script>