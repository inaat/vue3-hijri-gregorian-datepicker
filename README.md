# Vue3 Hijri Gregorian DatePicker

A comprehensive Vue 3 date picker component that seamlessly supports both Hijri (Islamic) and Gregorian calendars with the ability to switch between them on-the-fly.

[![npm version](https://img.shields.io/npm/v/vue3-hijri-gregorian-datepicker.svg)](https://www.npmjs.com/package/vue3-hijri-gregorian-datepicker)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## Features

✨ **Dual Calendar Support**: Switch seamlessly between Hijri (Islamic) and Gregorian calendars

⏰ **Time Selection**: Optional time picker with hours, minutes, and seconds

🌍 **Internationalization**: Built-in support for English and Arabic languages

🎨 **Theming**: Light and dark theme support

📅 **Flexible Formatting**: Customizable date and time format strings

♿ **Accessibility**: Keyboard navigation and screen reader support

🎯 **Vue 3 Ready**: Built with Vue 3 Composition API

📱 **Responsive**: Works great on mobile and desktop

🚫 **Min/Max Date**: Restrict selectable dates with `minDate` and `maxDate` props

⌨️ **Typeable Input**: Users can type the date directly in the input field (set `readOnly="false"`) with live validation

## Installation

```bash
npm install vue3-hijri-gregorian-datepicker
```

## Quick Start

```vue
<template>
  <div>
    <DatePicker v-model="selectedDate" />
    <p>Selected Date: {{ selectedDate.date }}</p>
    <p>Calendar Type: {{ selectedDate.type }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({
  date: '22-11-2025',
  type: 'gregorian'
});
</script>
```

## Usage Examples

### Basic Gregorian Calendar

```vue
<template>
  <DatePicker
    v-model="selectedDate"
    initialType="gregorian"
    language="en"
  />
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({
  date: '',
  type: 'gregorian'
});
</script>
```

### Hijri Calendar with Arabic Language

```vue
<template>
  <DatePicker
    v-model="selectedDate"
    initialType="hijri"
    language="ar"
  />
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({
  date: '',
  type: 'hijri'
});
</script>
```

### With Time Picker

```vue
<template>
  <DatePicker
    v-model="selectedDate"
    :withTime="true"
    initialType="gregorian"
  />
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({
  date: '',
  type: 'gregorian'
});
</script>
```

### Dark Theme

```vue
<template>
  <DatePicker
    v-model="selectedDate"
    :darkTheme="true"
  />
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({
  date: '',
  type: 'gregorian'
});
</script>
```

### Custom Date Format

```vue
<template>
  <DatePicker
    v-model="selectedDate"
    format="DD/MM/YYYY"
    initialType="gregorian"
  />
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({
  date: '',
  type: 'gregorian'
});
</script>
```

### With minDate and maxDate

```vue
<template>
  <!-- Only allow dates between 1 March 2025 and 30 June 2025 -->
  <DatePicker
    v-model="selectedDate"
    initialType="gregorian"
    minDate="01-03-2025"
    maxDate="30-06-2025"
  />

  <!-- Only allow future dates -->
  <DatePicker
    v-model="futureDate"
    initialType="gregorian"
    :minDate="todayDate"
  />

  <!-- Only allow past dates up to today -->
  <DatePicker
    v-model="pastDate"
    initialType="gregorian"
    :maxDate="todayDate"
  />
</template>

<script setup>
import { ref, computed } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({ date: '', type: 'gregorian' });
const futureDate = ref({ date: '', type: 'gregorian' });
const pastDate = ref({ date: '', type: 'gregorian' });

const todayDate = computed(() => {
  const today = new Date();
  const day = String(today.getDate()).padStart(2, '0');
  const month = String(today.getMonth() + 1).padStart(2, '0');
  const year = today.getFullYear();
  return `${day}-${month}-${year}`;
});
</script>
```

### Typeable Input (Keyboard Entry)

Set `readOnly="false"` to let users type the date directly. The input validates as they type — showing a red border on invalid or out-of-range values — and commits on **Enter** or **blur**. The calendar icon still opens the picker.

```vue
<template>
  <DatePicker
    v-model="selectedDate"
    :readOnly="false"
    initialType="gregorian"
  />
</template>

<script setup>
import { ref } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

const selectedDate = ref({ date: '', type: 'gregorian' });
</script>
```

Accepted typed formats:

- **Gregorian**: `DD-MM-YYYY`, `YYYY-MM-DD`, `MM/DD/YYYY`, `DD/MM/YYYY` (with optional ` HH:mm:ss` when `withTime` is enabled)
- **Hijri**: `iDD-iMM-iYYYY`, `iYYYY/iMM/iDD`, `iDD/iMM/iYYYY`

### Complete Example with All Options

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
    </div>

    <DatePicker
      :initialType="initialType"
      :withTime="withTime"
      v-model="selectedDate"
      :language="language"
      :darkTheme="darkTheme"
      :disabled="disabled"
      :readOnly="readOnly"
      :format="customFormat"
      placeholder="Select a date"
      @cancel="onCancel"
    />

    <!-- Display the selected date and type -->
    <div class="output">
      <p>Selected Date: {{ selectedDate.date }}</p>
      <p>Calendar Type: {{ selectedDate.type }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import DatePicker from 'vue3-hijri-gregorian-datepicker';
import 'vue3-hijri-gregorian-datepicker/dist/style.css';

// Reactive state
const darkTheme = ref(false);
const disabled = ref(false);
const withTime = ref(false);
const readOnly = ref(true);
const initialType = ref('gregorian');
const language = ref('en');
const customFormat = ref('');

const selectedDate = ref({
  date: new Date().toISOString().split('T')[0],
  type: initialType.value
});

// Watch initialType and update selectedDate
watch(initialType, (newType) => {
  selectedDate.value = {
    ...selectedDate.value,
    type: newType
  };
});

const onCancel = () => {
  console.log('Date picker cancelled');
};
</script>

<style>
.controls {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 1rem;
}

.controls label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.output {
  margin-top: 1rem;
  padding: 1rem;
  background-color: #f5f5f5;
  border-radius: 4px;
}
</style>
```

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `modelValue` | Object | `{ date: '', type: 'gregorian' }` | The selected date and calendar type |
| `initialType` | String | `'gregorian'` | Initial calendar type (`'gregorian'` or `'hijri'`) |
| `withTime` | Boolean | `false` | Enable time selection (hours, minutes, seconds) |
| `language` | String | `'en'` | UI language (`'en'` for English or `'ar'` for Arabic) |
| `format` | String | `''` | Custom date/time format string (uses moment.js format) |
| `disabled` | Boolean | `false` | Disable the date picker |
| `readOnly` | Boolean | `true` | Make the input field read-only. Set to `false` to allow the user to type the date directly (with live validation) |
| `placeholder` | String | `'Select date'` | Placeholder text for the input field |
| `darkTheme` | Boolean | `false` | Enable dark theme |
| `minDate` | String | `null` | Minimum selectable date (`'DD-MM-YYYY'` for Gregorian, `'iDD-iMM-iYYYY'` for Hijri) |
| `maxDate` | String | `null` | Maximum selectable date (`'DD-MM-YYYY'` for Gregorian, `'iDD-iMM-iYYYY'` for Hijri) |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `update:modelValue` | `{ date: String, type: String }` | Emitted when a date is selected |
| `cancel` | - | Emitted when the picker is closed without selecting |

## Date Format Strings

### Gregorian Calendar Formats

The component uses [moment.js](https://momentjs.com/docs/#/displaying/format/) format tokens:

- `DD-MM-YYYY` - Day-Month-Year (default)
- `MM/DD/YYYY` - Month/Day/Year
- `YYYY-MM-DD` - Year-Month-Day (ISO format)
- `DD-MM-YYYY HH:mm:ss` - With time
- `YYYY-MM-DD HH:mm:ss` - ISO format with time

### Hijri Calendar Formats

For Hijri dates, use the `i` prefix:

- `iDD-iMM-iYYYY` - Hijri Day-Month-Year (default)
- `iYYYY/iMM/iDD` - Hijri Year/Month/Day
- `iDD-iMM-iYYYY HH:mm:ss` - With time

### Common Format Tokens

| Token | Output | Description |
|-------|--------|-------------|
| `YYYY` | 2025 | 4-digit year |
| `MM` | 01-12 | Month |
| `DD` | 01-31 | Day |
| `HH` | 00-23 | Hours (24-hour) |
| `mm` | 00-59 | Minutes |
| `ss` | 00-59 | Seconds |
| `iYYYY` | 1446 | Hijri year |
| `iMM` | 01-12 | Hijri month |
| `iDD` | 01-30 | Hijri day |

## Styling

The component comes with default styles that can be imported:

```javascript
import 'vue3-hijri-gregorian-datepicker/dist/style.css';
```

### Custom Styling

You can override the default styles using CSS custom properties or by targeting the component classes:

```css
/* Custom theme colors */
.dp__theme_light {
  --dp-primary-color: #1976d2;
  --dp-background-color: #ffffff;
  --dp-text-color: #212121;
}

.dp__theme_dark {
  --dp-primary-color: #90caf9;
  --dp-background-color: #1e1e1e;
  --dp-text-color: #ffffff;
}
```

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Dependencies

- Vue 3.x
- moment-hijri
- moment
- date-fns

## Development

```bash
# Clone the repository
git clone https://github.com/inaat/vue3-hijri-gregorian-datepicker.git

# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**Inayat Ullah**

## Links

- [GitHub Repository](https://github.com/inaat/vue3-hijri-gregorian-datepicker)
- [NPM Package](https://www.npmjs.com/package/vue3-hijri-gregorian-datepicker)
- [Report Issues](https://github.com/inaat/vue3-hijri-gregorian-datepicker/issues)

## Acknowledgments

- Built with Vue 3
- Uses moment-hijri for Hijri calendar calculations
- Inspired by the need for dual calendar support in Vue applications

---

Made with ❤️ for the Vue community
