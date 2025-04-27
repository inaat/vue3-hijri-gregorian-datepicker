
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
    <DatePicker 
     :initialType="calendarType" 
      :withTime="true" 
      v-model="selectedDate" 
      :language="'en'" 
    
      :darkTheme="false"
    />
    />
    <div v-if="selectedDate">
        <p>Selected Date: {{ selectedDate.date }}</p>
        <p>Calendar Type: {{ selectedDate.type }}</p>
    </div>
</template>

<script>
  import 'vue3-hijri-gregorian-datepicker/dist/style.css';
  import DatePicker from 'vue3-hijri-gregorian-datepicker';

export default {
  components: {
    DatePicker,
  },
  data() {
    return {
        calendarType: 'gregorian',
      selectedDate: {
        date: this.getCurrentDateTime(),
        type: 'gregorian',
      },
    };
  },
};
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
