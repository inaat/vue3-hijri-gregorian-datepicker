
# vue3-hijri-gregorian-datepicker

vue3-hijri-gregorian-datepicker is a Vue 3 date picker component that supports both Hijri (Islamic) and Gregorian calendars. The component allows users to select a date and time, and provides options to switch between the two calendar systems.

## Features

- Hijri (Islamic) and Gregorian calendar support
- Time selection (hours, minutes, seconds)
- Easy to switch between Hijri and Gregorian calendars
- Configurable in English and Arabic languages
- Customizable date and time formatting

## Installation

To install vue3-hijri-gregorian-datepicker, you can use npm:

```bash
npm install vue3-hijri-gregorian-datepicker
```

## Usage

Here is a basic example of how to use the vue3-hijri-gregorian-datepicker in your Vue 3 project:

```vue
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
const initialType = ref('gregorian'); // Default to Hijri
const language = ref('en');
const customFormat = ref('');
const selectedFormat = ref('');
const selectedDate = ref({
  date:new Date().toISOString().split('T')[0],//'28-10-1446
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

```

## Props

- `initialType` (String): Specifies the initial calendar type. Can be 'gregorian' or 'hijri'. Default is 'gregorian'.
- `withTime` (Boolean): Enables time selection (hours, minutes, seconds). Default is `false`.
- `modelValue` (Object): The selected date and time value in UTC format , Calender type.
- `language` (String): The language for the UI. Can be 'en' for English or 'ar' for Arabic. Default is 'en'.
- `format` (String): The format for displaying the date and time.
- `disabled` (Boolean): Disables the date picker. Default is `false`.
- `readOnly` (Boolean): Makes the date picker read-only. Default is `true`.
- `darkTheme` (Boolean): The default theme is light . Default is `false`.
- `placeholder` (String): Placeholder text for the date input field. Default is 'Select date'.
## Events

- `update:modelValue`: Emits the selected date and time value in UTC format.
- `cancel`: Emits when the date picker is closed without confirming a date.

## License

This project is licensed under the MIT License.
