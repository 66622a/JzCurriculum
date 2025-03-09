<template>
  <div v-if="data">
    <h3 class="text-lg font-semibold mb-4">{{ data.Class }} 课程表</h3>
    
    <div class="overflow-x-auto">
      <table class="w-full border-collapse table-auto">
        <thead>
          <tr>
            <th class="header-cell">时段</th>
            <th class="header-cell">科目</th>
            <th class="header-cell">教师</th>
            <th class="header-cell">时间</th>
          </tr>
        </thead>
        
        <tbody>
          <tr v-for="(section, index) in data.Sections" :key="section.SectionId">
            <td class="time-cell whitespace-nowrap">{{ getSectionName(index) }}</td>
            <td class="lesson-cell">{{ section.Dts[0]?.Subject || "-" }}</td>
            <td class="lesson-cell">{{ section.Dts[0]?.Teacher || "-" }}</td>
            <td class="time-cell whitespace-nowrap">{{ formatTime(section.StartTime, section.EndTime) }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.header-cell {
  @apply px-4 py-2 text-center bg-gray-100 font-medium text-gray-700 border-b-2 border-gray-200;
}

.time-cell {
  @apply px-4 py-2 text-center text-gray-600 whitespace-nowrap;
  width: 30%;
}

.lesson-cell {
  @apply px-4 py-2 text-center text-gray-800;
}

table {
  @apply min-w-full;
  text-align: center;
}

tr {
  @apply border-t border-gray-200;
}

td {
  @apply border-b border-gray-200;
}

@media (max-width: 640px) {
  .header-cell,
  .time-cell,
  .lesson-cell {
    @apply px-2 py-1 text-sm;
  }
}

@media (prefers-color-scheme: dark) {
  .main-container {
    background-color: #1a202c;
  }
  .content-box {
    background: #2d3748;
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.8);
  }
  .footer-info {
    color: #a0aec0;
  }
  .section-comment {
    color: #cbd5e0;
  }
  /* 针对 Element Plus 组件的暗黑模式调整 */
  .el-select .el-input,
  .el-cascader .el-input,
  .el-date-picker .el-input__inner,
  .el-input__inner {
    background-color: #2d3748 !important;
    color: #e2e8f0 !important;
    border: 1px solid #4a5568 !important;
  }
  .el-select-dropdown,
  .el-cascader-panel,
  .el-picker-panel {
    background-color: #2d3748;
    color: #e2e8f0;
    border: 1px solid #4a5568;
  }
}


</style>

<script>
export default {
  props: {
    data: {
      type: Object,
      default: null
    }
  },
  methods: {
    formatTime(start, end) {
      return `${start.slice(0, 5)} - ${end.slice(0, 5)}`
    },
    getSectionName(index) {
      const sections = [
        '上午第一节', '上午第二节', '上午第三节', 
        '上午第四节', '上午第五节', '下午第一节',
        '下午第二节', '下午第三节' 
      ]
      return sections[index] || `第${index+1}节`
    }
  }
}
</script>