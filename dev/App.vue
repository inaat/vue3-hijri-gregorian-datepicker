<template>
  <div id="app">
    <!-- Controls -->
    <div class="controls">
      <label>
        <input type="checkbox" v-model="darkTheme" /> Dark Theme
      </label>
      <label>
        <input type="checkbox" v-model="disabled" /> Disabled
      </label>
      <label>
        <input type="checkbox" v-model="withTime" /> With Time
      </label>
      <label>
        <input type="checkbox" v-model="readOnly" /> Read Only
      </label>
      <label>
        Initial Type:
        <select v-model="initialType">
          <option value="gregorian">Gregorian</option>
          <option value="hijri">Hijri</option>
        </select>
      </label>
      <label>
        Language:
        <select v-model="language">
          <option value="en">English</option>
          <option value="ar">Arabic</option>
        </select>
      </label>
      <label>
        Predefined Formats:
        <select v-model="selectedFormat">
          <option v-for="format in availableFormats" :key="format" :value="format">
            {{ format }}
          </option>
        </select>
      </label>
      <label>
        Custom Format:
        <input
          type="text"
          v-model="customFormat"
          placeholder="Enter custom format (optional)"
        />
      </label>
    </div>
    
    <DatePicker 
      :initialType="initialType" 
      :withTime="withTime" 
      v-model="selectedDate" 
      cal
      :language="language" 
      :darkTheme="darkTheme"
      :disabled="disabled"
      :readOnly="readOnly"
      :format="actualFormat"
    />
    
    <!-- Display the selected date and type -->
    <p>Selected Date: {{ selectedDate.date }}</p>
    <p>Calendar Type: {{ selectedDate.type }}</p>
  </div>
</template><script setup>
import { ref, computed, watch } from 'vue';
import DatePicker from '../src/components/DatePicker.vue';

// Reactive state
const darkTheme = ref(false);
const disabled = ref(false);
const withTime = ref(false);
const readOnly = ref(false);
const initialType = ref('hijri'); // Default to Hijri
const language = ref('en');
const customFormat = ref('');
const selectedFormat = ref('');
const selectedDate = ref({
  date: new Date().toISOString().split('T')[0],//'28-10-1446
  type: initialType.value
});

// Watch initialType and update selectedDate
watch(initialType, (newType) => {
  selectedDate.value = {
    ...selectedDate.value,
    type: newType
  };
});

// Format options
const gregorianFormats = [
 
  'yyyy-MM-dd',
  'MM/dd/yyyy',
  'dd-MM-yyyy',
  'dd/MM/yyyy',
  'yyyy-MM-dd HH:mm:ss',
  'MM/dd/yyyy HH:mm:ss',
  'dd-MM-yyyy HH:mm:ss',
  'dd/MM/yyyy HH:mm:ss',
];

const hijriFormats = [
  'iYYYY-iMM-iDD',
  'iDD-iMM-iYYYY',
  'iYYYY/iMM/iDD',
  'iDD/iMM/iYYYY',
  'iYYYY-iMM-iDD HH:mm:ss',
  'iDD-iMM-iYYYY HH:mm:ss',
  'iYYYY/iMM/iDD HH:mm:ss',
  'iDD/iMM/iYYYY HH:mm:ss',
];

const availableFormats = computed(() => 
  initialType.value === 'hijri' ? hijriFormats : gregorianFormats
);

const actualFormat = computed(() => 
  customFormat.value || selectedFormat.value || ''
);

watch(selectedFormat, (newFormat) => {
  if (newFormat) customFormat.value = newFormat;
});
</script>