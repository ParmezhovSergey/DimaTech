<template>
  <div class="page">
    <div class="text">Круговая диаграмма</div>
    <div class="diagram">
      <div class="form">
        <FormComponent
          :key="entries.id"
          :entries="entries"
          @add="addEntry"
          @edit="editEntry"
          @delete="deleteEntry"
        />
      </div>

      <div class="diagram-container">
        <PieChart :chart-data="chartData" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PieChart from "./../components/PieChart.vue";
import FormComponent from "./../components/Form.vue";

const entries = ref([
  { id: 1, name: "Сектор-1", value: 25, color: "red" },
  { id: 2, name: "Сектор-2", value: 25, color: "yellow" },
  { id: 3, name: "Сектор-3", value: 25, color: "green" },
]);

const chartData = computed(() => ({
  labels: entries.value.map((e) => e.name),
  datasets: [
    {
      label: "Распределение",
      data: entries.value.map((e) => e.value),
      backgroundColor: entries.value.map((e) => e.color),
    },
  ],
}));

const addEntry = (newEntry) => {
  entries.value.push({ ...newEntry });
};

const editEntry = (updatedEntry) => {
  const newUsers = entries.value.map((obj) => {
    if (obj.id === updatedEntry.id) {
      return updatedEntry;
    }
    return obj;
  });
  entries.value = newUsers;
};

const deleteEntry = (id) => {
  entries.value = entries.value.filter((p) => p.id !== id);
};
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  align-items: center; /* Горизонтальное центрирование */
  width: 100%;
}
.diagram {
  display: flex;
  margin-top: 20px;
  border-top: 1px solid gray;
  padding-top: 20px;
  align-items: flex-start;
}
.text {
  font-size: 30px;
  font-weight: 600;
  margin-top: 20px;
  margin-right: 450px;
}
.form {
  width: 50%;
}
.diagram-container {
   width: 50%;
}
.diagram-container canvas {
}
</style>
