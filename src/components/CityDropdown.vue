<script setup>
import { ref, defineProps, defineEmits } from 'vue';
import { Listbox, ListboxButton, ListboxOptions, ListboxOption } from '@headlessui/vue';

// Props: modelValue (for v-model binding)
const props = defineProps({
  modelValue: String
});

// Emits event for v-model
const emit = defineEmits(["update:modelValue"]);

// List of 10 US cities
const cities = ref([
  "New York", "Los Angeles", "Chicago", "Houston", "Phoenix",
  "Philadelphia", "San Antonio", "San Diego", "Dallas", "San Jose"
]);
</script>

<template>
  <div class="flex items-center gap-4">
    <label class="w-32 text-gray-700 font-medium">City:</label>
    <Listbox :modelValue="props.modelValue" @update:modelValue="emit('update:modelValue', $event)">
      <div class="relative w-full">
        <ListboxButton class="w-full border border-gray-300 rounded-lg p-3 bg-white text-left focus:ring-2 focus:ring-blue-400 transition-all">
          {{ props.modelValue || "Select a city" }}
        </ListboxButton>
        <ListboxOptions class="absolute z-10 w-full bg-white border border-gray-300 rounded-lg shadow-md mt-1 max-h-48 overflow-y-auto scrollbar-thin scrollbar-thumb-gray-400 scrollbar-track-gray-100">
          <ListboxOption v-for="city in cities" :key="city" :value="city" class="cursor-pointer p-3 hover:bg-gray-100 transition-all">
            {{ city }}
          </ListboxOption>
        </ListboxOptions>
      </div>
    </Listbox>
  </div>
</template>

<style>
/* Custom scrollbar for CityDropdown */
.scrollbar-thin::-webkit-scrollbar {
  width: 6px;
}

.scrollbar-thin::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.scrollbar-thin::-webkit-scrollbar-thumb {
  background: #a0a0a0;
  border-radius: 5px;
}

.scrollbar-thin::-webkit-scrollbar-thumb:hover {
  background: #808080;
}
</style>
