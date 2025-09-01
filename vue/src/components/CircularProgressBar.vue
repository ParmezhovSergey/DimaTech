<template>
  <div class="circular-progress">
    <svg :width="size" :height="size" viewBox="0 0 100 100">
      <defs>
        <linearGradient id="progressGradient" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" stop-color="red" />
          <stop offset="100%" stop-color="green" />
        </linearGradient>
      </defs>
      <defs>
        <linearGradient id="circleGradient" x1="0%" y1="100%" x2="0%" y2="0%">
          <stop offset="48%" stop-color="red" />
          <stop offset="52%" stop-color="green" />
        </linearGradient>
      </defs>
      <circle
        v-if="type === 'circle'"
        class="circle"
        :class="status"
        :stroke-dasharray="strokeDasharray"
        :stroke-dashoffset="strokeDashoffset"
        :stroke-width="strokeWidth"
        fill="none"
        r="45"
        cx="50"
        cy="50"
        transform="rotate(-90 50 50)"
        stroke="url(#circleGradient)"
      />
      <path
        v-else
        class="circle"
        :class="status"
        :stroke-dasharray="strokeDasharray1"
        :stroke-dashoffset="strokeDashoffset1"
        :stroke-width="strokeWidth"
        fill="none"
        d="M18,81.7 A45,45 0 1,1 82,81.7"
        stroke="url(#progressGradient)"
      />

      <text
        v-if="
          (status !== 'success' &&
            status !== 'warning' &&
            status !== 'error') ||
          value < 100
        "
        x="50%"
        y="50%"
        dominant-baseline="middle"
        text-anchor="middle"
        class="percentage"
      >
        {{ value }}%
      </text>
      <path
        v-if="status === 'success' && value == 100"
        d="M45,47 L50,55 L60,40"
        fill="none"
        stroke="green"
        stroke-width="2"
      />
      <circle
        v-if="status === 'warning' && value == 100"
        cx="50"
        cy="50"
        r="7"
        fill="orange"
      />
      <text
        v-if="status === 'warning' && value == 100"
        x="50%"
        y="50%"
        dominant-baseline="middle"
        text-anchor="middle"
        class="warning-text"
      >
        !
      </text>
      <path
        v-if="status === 'error' && value == 100"
        d="M45,45 L55,55 M45,55 L55,45"
        fill="none"
        stroke="red"
        stroke-width="2"
      />
    </svg>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";

const props = defineProps({
  value: {
    type: Number,
    required: true,
    validator: (val) => val >= 0 && val <= 100,
  },
  size: {
    type: Number,
    default: 200,
  },
  strokeWidth: {
    type: Number,
    default: 7,
  },
  status: {
    type: String,
    default: "in-progress",
    validator: (val) =>
      ["in-progress", "success", "warning", "error"].includes(val),
  },
  type: {
    type: String,
    default: "circle",
    validator: (val) => ["circle", "dashboard"].includes(val),
  },
});

const circumference = 2 * Math.PI * 45;
const strokeDasharray = circumference;
const strokeDashoffset = computed(() => {
  return circumference - (props.value / 100) * circumference;
});

const radius = 45; // Радиус дуги
const arcLength = (3 / 2) * Math.PI * radius; // Длина дуги для 270 градусов
const strokeDasharray1 = arcLength;
const strokeDashoffset1 = computed(() => {
  return arcLength - (props.value / 100) * arcLength;
});

watch(
  () => props.value,
  (newValue, oldValue) => {
    if (newValue !== oldValue) {
      // Анимация изменения значения
      const animationDuration = 500; // Длительность анимации в миллисекундах
      const startTime = Date.now();
      const interval = setInterval(() => {
        const elapsedTime = Date.now() - startTime;
        const progress = Math.min(elapsedTime / animationDuration, 1);
        const currentValue = oldValue + (newValue - oldValue) * progress;
        strokeDashoffset.value =
          circumference - (currentValue / 100) * circumference;
        if (progress >= 1) {
          clearInterval(interval);
        }
      }, 10);
    }
  }
);
</script>

<style scoped>
.circle {
  transition: stroke-dashoffset 0.5s ease-in-out;
}

.success {
  stroke: green;
}

.warning {
  stroke: orange;
}

.error {
  stroke: red;
}

.percentage {
  font-size: 20px;
  fill: #333;
}
.warning-text {
  font-size: 10px; /* Уменьшенный размер */
  fill: white; /* Белый цвет */
}
</style>
