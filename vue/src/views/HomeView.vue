<template>
  <div class="demo">
    <h1>Прогресс бар</h1>
    <div class="progress-bar">
      <CircularProgressBar :value="progress" :status="status" :type="type" />
    </div>
    <div class="controls">
      <label class="controls">
        Progress:
        <input type="range" v-model="progress" min="0" max="100" />
      </label>
      <label>
        Status:
        <select v-model="status">
          <option value="in-progress">In Progress</option>
          <option value="success">Success</option>
          <option value="warning">Warning</option>
          <option value="error">Error</option>
        </select>
      </label>
      <label>
        Type:
        <select v-model="type">
          <option value="circle">Circle</option>
          <option value="dashboard">Dashboard</option>
        </select>
      </label>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import CircularProgressBar from "./../components/CircularProgressBar.vue";

const progress = ref(10);
const status = ref("in-progress");
const type = ref("circle");
</script>

<style scoped>
.demo {
  text-align: center;
  padding: 20px;
}

.progress-bar {
  margin: 20px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 20px;
}
</style>
