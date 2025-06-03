<template>
  <div id="app">

  //import DatePickerWrapper from './DatePickerWrapper.vue';
  <DatePickerWrapper 
      :withTime="false" 
      @updateDate="event_end_date_g"  
      v-model="end_date_g" 
    />
    <p>Selected Date: {{ end_date_g }}</p>
    <hr>


    ////DatePicker from '../src/components/DatePicker.vue'; direct call 
    <DatePicker 
    :key="internalValue.date" 
      :initialType="calendarType"
      :withTime="withTime"
      v-model="internalValue"
      :language="'en'"
      :darkTheme="darkMode"
      :disabled="disabled"
    />
{{ internalValue }}
  </div>
</template><script setup>
import { ref, computed, watch } from 'vue';
import DatePicker from '../src/components/DatePicker.vue';

import DatePickerWrapper from './DatePickerWrapper.vue';
const calendarType = ref('gregorian'); // Default to Gregorian

const end_date_g = ref(null); // Initialize end_date_g as a ref



const event_end_date_g = (date) => {
   // alert( formatDate(date));
   end_date_g.value =date;
   // checkEndDates();
};
const darkMode = false;

// Internal state for calendarType

// Internal state: Convert the date string into an object
const internalValue = ref({
  date: null,
  type: calendarType.value
});
</script>