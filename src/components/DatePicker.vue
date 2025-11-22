<template>
  <div ref="datepickerRef" :class="['datepicker','dp__main', themeClass]">
    <!-- Input field to trigger the date picker -->
    <div class="datepicker-input" @click="openDatePicker">
      <input class="dp__pointer dp__input_readonly dp__input dp__input_icon_pad dp__input_focus dp__input_reg" 
        type="text" :value="formattedDate" :readOnly="readOnly" :placeholder="placeholder"
        :disabled="disabled" />
      <span class="datepicker-icon">
        <CalendarIcon className="dp__input_icon dp__input_icons" :size="48" />
      </span>
    </div>

    <!-- Date picker dialog with transition -->
    <transition name="fade">
      <div ref="datepickerDialogRef" :class="['datepicker-overlay', themeClass, 'dp__menu dp__menu_index']" v-if="showPicker">
        <div class="datepicker-dialog">
          <div class="calendar">
            <div class="calendar-header">
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-if="isYearSelection || isMonthSelection"
                @click="prevYearsOrMonths"><span class="dp__inner_nav"><BackArrowIcon className="custom-arrow-icon" :size="40" /></span></DatePickerButton>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-else @click="prevMonth"><span
                  class="dp__inner_nav"><BackArrowIcon className="custom-arrow-icon" :size="40" /></span></DatePickerButton>
              <span @click="toggleYearSelection">{{ currentMonth }} {{ currentYear }}</span>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-if="isYearSelection || isMonthSelection"
                @click="nextYearsOrMonths"><span class="dp__inner_nav"><ForwardArrowIcon className="custom-forward-icon" :size="40" /></span></DatePickerButton>
              <DatePickerButton className="dp__btn dp--arrow-btn-nav" v-else @click="nextMonth"><span
                  class="dp__inner_nav"><ForwardArrowIcon className="custom-forward-icon" :size="40" /></span></DatePickerButton>
            </div>

            <!-- Year Selection -->
            <transition name="fade">
              <div v-if="isYearSelection" class="year-selection">
                <div v-for="year in visibleYears" :key="year" :class="{ selected: year === currentYearNumber }"
                  @click="selectYear(year)">
                  {{ year }}
                </div>
              </div>
            </transition>

            <!-- Month Selection -->
            <transition name="fade">
              <div v-if="isMonthSelection" class="month-selection">
                <div v-for="(month, index) in months" :key="month" :class="{ selected: index === currentMonthIndex }"
                  @click="selectMonth(index)">
                  {{ month }}
                </div>
              </div>
            </transition>

            <!-- Calendar Grid -->
            <transition name="fade">
              <div v-if="!isYearSelection && !isMonthSelection" class="calendar-grid">
                <div v-for="day in daysOfWeek" :key="day" class="calendar-day-header">
                  {{ day }}
                </div>
                <div v-for="day in daysInMonth" :key="day.date"
                  :class="['calendar-day', { selected: isSelected(day.date) }]" @click="selectDate(day.date)">
                  {{ day.day }}
                </div>
              </div>
            </transition>
          </div>

          <!-- Time Selection (if enabled) -->
          <div v-if="withTime">
            <button type="button" class="dp__btn dp__button" aria-label="Close time Picker" @click="showTimeArea" tabindex="0">
              <ClockIcon className="custom-clock-icon" :size="48" />
            </button>
            <div class="dp--tp-wrap">
              <div v-if="withTime && showTime" role="dialog" class="dp__overlay dp--overlay-absolute" aria-label="Time picker" tabindex="0">
                <div class="dp__overlay_container dp__container_flex dp__time_picker_overlay_container" style="display: flex">
                  <div class="dp__overlay_row dp__flex_row">
                    <div class="dp__time_input">
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="hours-time-inc-btn-0"
                          aria-label="Increment hours" tabindex="0" @click="incrementHours">
                          <UpArrowIcon className="custom-up-arrow" :size="40" />
                        </button>
                        <button type="button" aria-label="19-Open hours overlay"
                          class="dp__time_display dp__time_display_block dp--time-overlay-btn" tabindex="0"
                          data-test="hours-toggle-overlay-btn-0">
                          {{ formattedHour }}
                        </button>
                        <button type="button" class="dp__btn dp__inc_dec_button"
                          data-test="hours-time-dec-btn-0" aria-label="Decrement hours" tabindex="0"
                          @click="decrementHours">
                          <DownArrowIcon className="custom-down-arrow" :size="40" />
                        </button>
                      </div>
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        :
                      </div>
                      <div class="dp__time_col dp__time_col_block dp__time_col_reg_block">
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="minutes-time-inc-btn-0"
                          aria-label="Increment minutes" tabindex="0" @click="incrementMinutes">
                          <UpArrowIcon className="custom-up-arrow" :size="40" />
                        </button>
                        <button type="button" aria-label="56-Open minutes overlay"
                          class="dp__time_display dp__time_display_block dp--time-overlay-btn" tabindex="0"
                          data-test="minutes-toggle-overlay-btn-0">
                          {{ formattedMinute }}
                        </button>
                        <button type="button" class="dp__btn dp__inc_dec_button" data-test="minutes-time-dec-btn-0"
                          aria-label="Decrement minutes" tabindex="0" @click="decrementMinutes">
                          <DownArrowIcon className="custom-down-arrow" :size="40" />
                        </button>
                      </div>
                    </div>
                  </div>
                  <button type="button" class="dp__btn dp__button" aria-label="Close time Picker" @click="hideTimeArea" tabindex="0">
                    <CalendarIcon className="dp__icon" :size="8" />
                  </button>
                </div>
              </div>
            </div>
          </div>
          
          <!-- Actions -->
          <div class="dp__action_row">
            <div class="dp__selection_preview" title="">{{ formattedDate }}</div>
            <div class="dp__action_buttons">
              <transition name="fade">
                <DatePickerButton className="dp__action_button dp__action_select" @click="confirmSelection">{{
                  translations.ok }}</DatePickerButton>
              </transition>
              <transition name="fade">
                <DatePickerButton className="dp__action_button dp__action_cancel" @click="cancelSelection">{{
                  translations.cancel }}</DatePickerButton>
              </transition>
              <transition name="fade">
                <DatePickerButton className="dp__action_button dp__action_select" @click="switchCalendar">{{
                  isHijri
                    ? translations.switchToGregorian
                    : translations.switchToHijri
                }}</DatePickerButton>
              </transition>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount, nextTick } from "vue";
import moment from "moment-hijri";
import { addMonths, subMonths } from "date-fns";
import DatePickerButton from "./common/DatePickerButton.vue";
import CalendarIcon from './common/CalendarIcon.vue';
import ClockIcon from './common/ClockIcon.vue';
import BackArrowIcon from './common/BackArrowIcon.vue';
import ForwardArrowIcon from './common/ForwardArrowIcon.vue';
import DownArrowIcon from './common/DownArrowIcon.vue';
import UpArrowIcon from './common/UpArrowIcon.vue';

const props = defineProps({
  initialType: {
    type: String,
    default: "gregorian",
  },
  withTime: {
    type: Boolean,
    default: false,
  },
  modelValue: {
    type: Object,
    default: () => ({ date: '', type: 'gregorian' }),
  },
  language: {
    type: String,
    default: "en",
  },
  format: {
    type: String,
    default: "", // Empty default to use automatic formatting
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  readOnly: {
    type: Boolean,
    default: true,
  },
  placeholder: {
    type: String,
    default: 'Select date',
  },
  darkTheme: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(["update:modelValue", "cancel"]);
const datepickerRef = ref(null);
const datepickerDialogRef = ref(null);

const themeClass = computed(() => props.darkTheme ? 'dp__theme_dark' : 'dp__theme_light');
const isHijri = ref(props.initialType === "hijri");
const showPicker = ref(false);
const showTime = ref(false);
const currentDate = ref(
  isHijri.value ? moment().startOf("day").toDate() : new Date()
);
const selectedDate = ref(currentDate.value);
const selectedHour = ref(currentDate.value.getHours());
const selectedMinute = ref(currentDate.value.getMinutes());
const selectedSecond = ref(currentDate.value.getSeconds());

const isYearSelection = ref(false);
const isMonthSelection = ref(false);

const yearRangeStart = ref(isHijri.value ? 1400 : 2000);
const formattedHour = computed(() => pad(selectedHour.value));
const formattedMinute = computed(() => pad(selectedMinute.value));

const formattedDate = computed(() => {
  if (!props.modelValue.date) return '';
  if (!selectedDate.value) return '';

  // Determine the output format based on calendar type and time inclusion
  let formatString;

  if (props.format) {
    // Use custom format if provided
    formatString = props.format;
  } else {
    // Use defaults based on calendar type and time setting
    if (isHijri.value) {
      formatString = props.withTime ? "iDD-iMM-iYYYY HH:mm:ss" : "iDD-iMM-iYYYY";
    } else {
      formatString = props.withTime ? "DD-MM-YYYY HH:mm:ss" : "DD-MM-YYYY";
    }
  }

  // Create a temporary date object with time components if enabled
  let dateToFormat = selectedDate.value;
  if (props.withTime && selectedDate.value) {
    dateToFormat = new Date(selectedDate.value);
    dateToFormat.setHours(
      selectedHour.value,
      selectedMinute.value,
      selectedSecond.value,
      0
    );
  }

  // Format the date using the appropriate method
  try {
    return moment(dateToFormat).format(formatString);
  } catch (e) {
    console.error("Error formatting date:", e);
    return dateToFormat ? dateToFormat.toLocaleDateString() : '';
  }
});

const parseInputDate = (dateString, calendarType) => {
  if (!dateString) return new Date();
  
  try {
    if (calendarType === "hijri") {
      // Handle Hijri date parsing
      const parsed = moment(dateString, "iDD-iMM-iYYYY");
      if (parsed.isValid()) {
        return parsed.toDate();
      }
      
      // Try other Hijri formats if the first one fails
      const formats = ["iYYYY/iMM/iDD", "iDD/iMM/iYYYY"];
      for (const fmt of formats) {
        const parsed = moment(dateString, fmt);
        if (parsed.isValid()) return parsed.toDate();
      }
      
      console.warn("Invalid Hijri date format, using current date");
      return moment().toDate();
    } else {
      // For Gregorian calendar, parse DD-MM-YYYY format first
      const formats = ["DD-MM-YYYY", "YYYY-MM-DD", "MM/DD/YYYY", "DD/MM/YYYY"];
      for (const fmt of formats) {
        const parsed = moment(dateString, fmt, true); // strict parsing
        if (parsed.isValid()) {
          return parsed.toDate();
        }
      }
      
      // Fallback to current date
      console.warn("Invalid Gregorian date format, using current date");
      return new Date();
    }
  } catch (e) {
    console.error("Error parsing date:", e);
    return new Date();
  }
};

const confirmSelection = async () => {
  if (!selectedDate.value) return;
  
  const finalDate = new Date(selectedDate.value);
  if (props.withTime) {
    finalDate.setHours(selectedHour.value, selectedMinute.value, selectedSecond.value, 0);
  } else {
    finalDate.setHours(0, 0, 0, 0);
  }
  
  const calendarType = isHijri.value ? "hijri" : "gregorian";
  
  // Determine output format - FIXED to use consistent DD-MM-YYYY
  let outputFormat;
  if (props.format) {
    outputFormat = props.format;
  } else {
    if (calendarType === "hijri") {
      outputFormat = props.withTime ? "iDD-iMM-iYYYY HH:mm:ss" : "iDD-iMM-iYYYY";
    } else {
      outputFormat = props.withTime ? "DD-MM-YYYY HH:mm:ss" : "DD-MM-YYYY";
    }
  }
  
  // Format the date using moment for consistency
  let formattedDateStr;
  try {
    formattedDateStr = moment(finalDate).format(outputFormat);
  } catch (e) {
    console.error("Error formatting output date:", e);
    formattedDateStr = finalDate.toISOString().split('T')[0];
  }
  
  emit("update:modelValue", {
    date: formattedDateStr,
    type: calendarType,
  });
  
  showPicker.value = false;
  document.removeEventListener('click', handleClickOutside);
};
const translations = computed(() => {
  return props.language === "ar"
    ? {
      ok: "موافق",
      cancel: "إلغاء",
      switchToGregorian: "التبديل إلى الميلادي",
      switchToHijri: "التبديل إلى الهجري",
      hh: "ساعة",
      mm: "دقيقة",
      ss: "ثانية",
      monthsGregorian: [
        "يناير",
        "فبراير",
        "مارس",
        "أبريل",
        "مايو",
        "يونيو",
        "يوليو",
        "أغسطس",
        "سبتمبر",
        "أكتوبر",
        "نوفمبر",
        "ديسمبر",
      ],
      monthsHijri: [
        "محرم",
        "صفر",
        "ربيع الأول",
        "ربيع الآخر",
        "جمادى الأولى",
        "جمادى الآخرة",
        "رجب",
        "شعبان",
        "رمضان",
        "شوال",
        "ذو القعدة",
        "ذو الحجة",
      ],
      daysOfWeek: [
        "الإثنين",
        "الثلاثاء",
        "الأربعاء",
        "الخميس",
        "الجمعة",
        "السبت",
        "الأحد",
      ],
    }
    : {
      ok: "Select",
      cancel: "Cancel",
      switchToGregorian: "Switch to Gregorian",
      switchToHijri: "Switch to Hijri",
      hh: "HH",
      mm: "MM",
      ss: "SS",
      monthsGregorian: [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ],
      monthsHijri: [
        "Muharram",
        "Safar",
        "Rabi I",
        "Rabi II",
        "Jumada I",
        "Jumada II",
        "Rajab",
        "Shaaban",
        "Ramadan",
        "Shawwal",
        "Dhu al-Qidah",
        "Dhu al-Hijjah",
      ],
      daysOfWeek: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
    };
});

const currentMonth = computed(() => {
  if (!selectedDate.value) return '';

  return isHijri.value
    ? translations.value.monthsHijri[moment(selectedDate.value).iMonth()]
    : translations.value.monthsGregorian[selectedDate.value.getMonth()];
});

const currentMonthIndex = computed(() => {
  if (!selectedDate.value) return -1;

  return isHijri.value
    ? moment(selectedDate.value).iMonth()
    : selectedDate.value.getMonth();
});

const currentYear = computed(() => {
  if (!selectedDate.value) return '';

  return isHijri.value
    ? moment(selectedDate.value).format("iYYYY")
    : moment(selectedDate.value).format("YYYY");
});

const currentYearNumber = computed(() => {
  if (!selectedDate.value) return -1;

  return isHijri.value
    ? parseInt(moment(selectedDate.value).format("iYYYY"))
    : selectedDate.value.getFullYear();
});

const visibleYears = ref([]);

const months = computed(() => {
  return isHijri.value
    ? translations.value.monthsHijri
    : translations.value.monthsGregorian;
});

const daysOfWeek = computed(() => {
  return translations.value.daysOfWeek;
});

const daysInMonth = computed(() => {
  if (!selectedDate.value) return [];
  
  const days = [];
  const startOfMonth = isHijri.value
    ? moment(selectedDate.value).startOf("iMonth")
    : new Date(
      selectedDate.value.getFullYear(),
      selectedDate.value.getMonth(),
      1
    );

  const startDay = isHijri.value
    ? (startOfMonth.isoWeekday() - 1)
    : (startOfMonth.getDay() + 6) % 7;

  for (let i = 0; i < startDay; i++) {
    days.push({ day: "", date: null });
  }

  const totalDays = isHijri.value
    ? moment(selectedDate.value).iDaysInMonth()
    : new Date(
      selectedDate.value.getFullYear(),
      selectedDate.value.getMonth() + 1,
      0
    ).getDate();

  for (let day = 1; day <= totalDays; day++) {
    const date = isHijri.value
      ? moment(startOfMonth).iDate(day).toDate()
      : new Date(startOfMonth.getFullYear(), startOfMonth.getMonth(), day);
    days.push({ day, date });
  }

  return days;
});

const pad = (value) => String(value).padStart(2, "0");

const prevMonth = () => {
  if (!selectedDate.value) return;
  
  selectedDate.value = isHijri.value
    ? moment(selectedDate.value).subtract(1, "iMonth").toDate()
    : subMonths(selectedDate.value, 1);
  updateDate();
};

const nextMonth = () => {
  if (!selectedDate.value) return;
  
  selectedDate.value = isHijri.value
    ? moment(selectedDate.value).add(1, "iMonth").toDate()
    : addMonths(selectedDate.value, 1);
  updateDate();
};

const selectYear = (year) => {
  if (!selectedDate.value) return;
  
  if (isHijri.value) {
    selectedDate.value = moment(selectedDate.value).iYear(year).toDate();
  } else {
    const newDate = new Date(selectedDate.value);
    newDate.setFullYear(year);
    selectedDate.value = newDate;
  }
  isYearSelection.value = false;
  isMonthSelection.value = true;
  updateDate();
};

const selectMonth = (monthIndex) => {
  if (!selectedDate.value) return;
  
  if (isHijri.value) {
    selectedDate.value = moment(selectedDate.value)
      .iMonth(monthIndex)
      .startOf("iMonth")
      .toDate();
  } else {
    const newDate = new Date(selectedDate.value);
    newDate.setMonth(monthIndex);
    selectedDate.value = newDate;
  }
  isMonthSelection.value = false;
  updateDate();
};

const selectDate = (date) => {
  if (date) {
    selectedDate.value = date;
    if (!props.withTime) {
      confirmSelection();
    }
  }
};

const showTimeArea = () => {
  showTime.value = true;
};

const hideTimeArea = () => {
  showTime.value = false;
};

const incrementHours = () => {
  if (!selectedDate.value) return;
  
  var hour = (selectedHour.value + 1) % 24;
  selectedHour.value = hour;
  
  const newDate = new Date(selectedDate.value);
  newDate.setHours(hour);
  selectedDate.value = newDate;
};

const decrementHours = () => {
  if (!selectedDate.value) return;
  
  var hour = (selectedHour.value - 1 + 24) % 24;
  selectedHour.value = hour;
  
  const newDate = new Date(selectedDate.value);
  newDate.setHours(hour);
  selectedDate.value = newDate;
};

const incrementMinutes = () => {
  if (!selectedDate.value) return;
  
  var minute = (selectedMinute.value + 1) % 60;
  selectedMinute.value = minute;
  
  const newDate = new Date(selectedDate.value);
  newDate.setMinutes(minute);
  selectedDate.value = newDate;
};

const decrementMinutes = () => {
  if (!selectedDate.value) return;
  
  var minute = (selectedMinute.value - 1 + 60) % 60;
  selectedMinute.value = minute;
  
  const newDate = new Date(selectedDate.value);
  newDate.setMinutes(minute);
  selectedDate.value = newDate;
};

const switchCalendar = () => {
  isHijri.value = !isHijri.value;
  const calendarType = isHijri.value ? "hijri" : "gregorian";

  // Update year range start based on the current selected date
  const currentYear = isHijri.value
    ? parseInt(moment(selectedDate.value).format("iYYYY"))
    : selectedDate.value.getFullYear();

  yearRangeStart.value = Math.floor(currentYear / 10) * 10;

  // Emit the current formatted date with the new calendar type
  emit("update:modelValue", {
    date: formattedDate.value,
    type: calendarType,
  });

  updateVisibleYears();
};

const isSelected = (date) => {
  if (!date || !selectedDate.value) return false;
  return date.toDateString() === selectedDate.value.toDateString();
};



const cancelSelection = () => {
  emit("cancel");
  showPicker.value = false;
  document.removeEventListener('click', handleClickOutside);
};

const toggleYearSelection = () => {
  isYearSelection.value = !isYearSelection.value;
  isMonthSelection.value = false;
  if (isYearSelection.value) {
    updateVisibleYears();
  }
};

const prevYearsOrMonths = () => {
  yearRangeStart.value -= 10;
  updateVisibleYears();
};

const nextYearsOrMonths = () => {
  yearRangeStart.value += 10;
  updateVisibleYears();
};

const updateDate = () => {
  if (selectedDate.value) {
    selectedDate.value = new Date(selectedDate.value);
  }
};

const updateVisibleYears = () => {
  visibleYears.value = Array.from(
    { length: 10 },
    (_, i) => yearRangeStart.value + i
  );
};

const handleClickOutside = (event) => {
  if (showPicker.value && 
      datepickerRef.value && 
      !datepickerRef.value.contains(event.target) &&
      (!datepickerDialogRef.value || !datepickerDialogRef.value.contains(event.target))) {
    cancelSelection();
  }
};

const openDatePicker = () => {
  if (props.disabled) return;
  showPicker.value = true;
  nextTick(() => {
    setTimeout(() => {
      document.addEventListener('click', handleClickOutside);
    }, 50);
  });
};

onMounted(() => {
  // Set calendar type first
  isHijri.value = props.initialType === "hijri" || (props.modelValue && props.modelValue.type === "hijri");

  if (props.modelValue && props.modelValue.date) {
    // Parse the input date based on the calendar type
    const calendarType = props.modelValue.type || props.initialType;
    selectedDate.value = parseInputDate(props.modelValue.date, calendarType);

    // Extract time components
    selectedHour.value = selectedDate.value.getHours();
    selectedMinute.value = selectedDate.value.getMinutes();
    selectedSecond.value = selectedDate.value.getSeconds();
  } else {
    // Default to the current date and time
    selectedDate.value = isHijri.value
      ? moment().startOf("day").toDate()  // Hijri default
      : new Date();                        // Gregorian default

    selectedHour.value = selectedDate.value.getHours();
    selectedMinute.value = selectedDate.value.getMinutes();
    selectedSecond.value = selectedDate.value.getSeconds();
  }

  // Set year range based on the selected date to show current year
  const currentYear = isHijri.value
    ? parseInt(moment(selectedDate.value).format("iYYYY"))
    : selectedDate.value.getFullYear();

  // Center the year range around the current year (show current year in middle)
  yearRangeStart.value = Math.floor(currentYear / 10) * 10;

  // Update visible years in the calendar
  updateVisibleYears();
});

onBeforeUnmount(() => {
  document.removeEventListener('click', handleClickOutside);
});
</script>