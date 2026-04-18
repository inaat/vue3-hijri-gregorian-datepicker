<template>
  <div id="app">
    <h1>DatePicker maxDate &amp; minDate Feature Tests</h1>

    <!-- Test 1: Without maxDate -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd;">
      <h3>Test 1: No maxDate (all dates selectable)</h3>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate1"
        :language="'en'"
      />
      <p><strong>Selected:</strong> {{ testDate1 }}</p>
    </div>

    <!-- Test 2: With maxDate = 26-06-2025 -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd; background: #f9f9f9;">
      <h3>Test 2: maxDate="26-06-2025"</h3>
      <p style="color: red;">Dates after 26 June 2025 should be DISABLED</p>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate2"
        :language="'en'"
        :maxDate="'26-06-2025'"
      />
      <p><strong>Selected:</strong> {{ testDate2 }}</p>
    </div>

    <!-- Test 3: maxDate = Today -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd;">
      <h3>Test 3: maxDate=Today ({{ todayDate }})</h3>
      <p style="color: red;">All future dates should be DISABLED</p>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate3"
        :language="'en'"
        :maxDate="todayDate"
      />
      <p><strong>Selected:</strong> {{ testDate3 }}</p>
    </div>

    <!-- Test 4: Dark theme with maxDate -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd; background: #333; color: white;">
      <h3>Test 4: Dark Theme + maxDate="31-12-2025"</h3>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate4"
        :language="'en'"
        :darkTheme="true"
        :maxDate="'31-12-2025'"
      />
      <p><strong>Selected:</strong> {{ testDate4 }}</p>
    </div>

    <!-- Test 5: With Time -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd;">
      <h3>Test 5: With Time + maxDate="30-06-2025"</h3>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="true"
        v-model="testDate5"
        :language="'en'"
        :maxDate="'30-06-2025'"
      />
      <p><strong>Selected:</strong> {{ testDate5 }}</p>
    </div>

    <!-- Test 6: With minDate only -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd; background: #f0fff0;">
      <h3>Test 6: minDate="01-01-2025"</h3>
      <p style="color: red;">Dates before 1 January 2025 should be DISABLED</p>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate6"
        :language="'en'"
        :minDate="'01-01-2025'"
      />
      <p><strong>Selected:</strong> {{ testDate6 }}</p>
    </div>

    <!-- Test 7: With both minDate and maxDate -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd; background: #fff0f0;">
      <h3>Test 7: minDate="01-03-2025" + maxDate="30-06-2025"</h3>
      <p style="color: red;">Only dates between 1 March 2025 and 30 June 2025 should be selectable</p>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate7"
        :language="'en'"
        :minDate="'01-03-2025'"
        :maxDate="'30-06-2025'"
      />
      <p><strong>Selected:</strong> {{ testDate7 }}</p>
    </div>

    <!-- Test 8: minDate = Today -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd; background: #f0f0ff;">
      <h3>Test 8: minDate=Today ({{ todayDate }}) — Future dates only</h3>
      <p style="color: red;">All past dates should be DISABLED</p>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate8"
        :language="'en'"
        :minDate="todayDate"
      />
      <p><strong>Selected:</strong> {{ testDate8 }}</p>
    </div>

    <!-- Test 9: Typeable input (readOnly=false) -->
    <div style="margin: 20px 0; padding: 20px; border: 1px solid #ddd; background: #fffbe6;">
      <h3>Test 9: Type the date directly (readOnly=false)</h3>
      <p>Type a date like <code>15-06-2025</code> then press Enter or click outside.</p>
      <DatePicker
        :initialType="'gregorian'"
        :withTime="false"
        v-model="testDate9"
        :language="'en'"
        :readOnly="false"
      />
      <p><strong>Selected:</strong> {{ testDate9 }}</p>
    </div>

    <hr>

    <!-- Original Tests -->
    <h2>Original Tests</h2>
    <DatePickerWrapper
      :withTime="false"
      @updateDate="event_end_date_g"
      v-model="end_date_g"
    />
    <p>Selected Date: {{ end_date_g }}</p>

    <hr>

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
</template>

<script setup>
import { ref, computed } from 'vue';
import DatePicker from '../src/components/DatePicker.vue';
import DatePickerWrapper from './DatePickerWrapper.vue';

// Test dates
const testDate1 = ref({ date: null, type: 'gregorian' });
const testDate2 = ref({ date: null, type: 'gregorian' });
const testDate3 = ref({ date: null, type: 'gregorian' });
const testDate4 = ref({ date: null, type: 'gregorian' });
const testDate5 = ref({ date: null, type: 'gregorian' });
const testDate6 = ref({ date: null, type: 'gregorian' });
const testDate7 = ref({ date: null, type: 'gregorian' });
const testDate8 = ref({ date: null, type: 'gregorian' });
const testDate9 = ref({ date: null, type: 'gregorian' });

// Get today's date in DD-MM-YYYY format
const todayDate = computed(() => {
  const today = new Date();
  const day = String(today.getDate()).padStart(2, '0');
  const month = String(today.getMonth() + 1).padStart(2, '0');
  const year = today.getFullYear();
  return `${day}-${month}-${year}`;
});

// Original test variables
const calendarType = ref('gregorian');
const end_date_g = ref(null);
const withTime = ref(false);
const darkMode = false;
const disabled = false;

const event_end_date_g = (date) => {
  end_date_g.value = date;
};

const internalValue = ref({
  date: null,
  type: calendarType.value
});
</script>
