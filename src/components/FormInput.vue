<script setup>
import { defineProps } from 'vue';

// Props: modelValue (for v-model binding), label, type
defineProps({
  modelValue: String,
  label: String,
  type: {
    type: String,
    default: "text"
  }
});
</script>

<template>
  <div class="flex items-center gap-4">
    <label class="w-32 text-gray-700 font-medium">{{ label }}:</label>
    <input
        :type="type"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        class="flex-1 border border-gray-300 rounded-lg p-3 focus:ring-2 focus:ring-blue-400 outline-none transition-all"
    />
  </div>
</template>
