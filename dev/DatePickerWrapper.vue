<!-- DatePickerWrapper.vue -->
<template>
  <div @click.stop.prevent>
    <DatePicker
      :key="internalValue.date" 
      :initialType="calendarType"
      :withTime="withTime"
      v-model="internalValue"
      :language="'en'"
      :darkTheme="darkMode"
      :disabled="disabled"
      
    />
    {{ internalValue.date }}
    <p>Calendar Type: {{ calendarType }}</p>
  </div>
</template>

<script setup>
import { ref, watch, defineProps, defineEmits } from 'vue';
import DatePicker from '../src/components/DatePicker.vue';

const emit = defineEmits(['update:modelValue', 'updateDate']);
const props = defineProps({
  modelValue: [String, null], // Parent passes only the date (string)
  withTime: { type: Boolean, default: false },
  disabled: { type: Boolean, default: false }
});

const darkMode = false;

// Internal state for calendarType
const calendarType = ref('gregorian'); // Default to Gregorian

// Internal state: Convert the date string into an object
const internalValue = ref({
  date: props.modelValue || null,
  type: calendarType.value
});

// Watch for changes in props.modelValue (from parent) and update internalValue
watch(() => props.modelValue, (newDate) => {
  if (newDate !== internalValue.value.date) {
    // Create a completely new object to ensure reactivity
    internalValue.value = {
      date: newDate,
      type: calendarType.value
    };
  }
}, { immediate: true });

// Watch for changes in internalValue and emit events
watch(internalValue, (val) => {
  // Update calendarType if the type changes
  if (val.type !== calendarType.value) {
    calendarType.value = val.type; // Update internal calendarType
  }
  
  // Emit the updated date to the parent
  emit('update:modelValue', val.date);
  emit('updateDate', val.date);
}, { deep: true });
</script>

