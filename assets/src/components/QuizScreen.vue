<script setup lang="ts">
defineProps({
  currentQuestion: {
    type: Object,
    required: true,
  },
  progressPercentage: {
    type: Number,
    required: true,
  },
  modelValue: {
    type: String as () => string | null,
    default: null,
  },
});

defineEmits(['update:modelValue', 'setAnswer']);
</script>

<template>
  <section class="body">
    <div class="body__progress">
      <div class="body__progress-fill" :style="{ width: progressPercentage + '%' }"></div>
    </div>
    <div class="body__title">
      <h2 class="body__title-question" :key="currentQuestion.question">
        {{ currentQuestion.question }}
      </h2>
    </div>
    <div class="body__options">
      <label
        v-for="(option, index) in currentQuestion.options"
        :key="index"
        class="body__options-item">
        <input
          type="radio"
          :id="'option' + index"
          name="question"
          :value="option"
          v-model="modelValue"
          @change="$emit('update:modelValue', option); $emit('setAnswer')" />
        <span>{{ option }}</span>
      </label>
    </div>
  </section>
</template>
