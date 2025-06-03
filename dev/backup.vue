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
      // Use ddd for abbreviated day name (Mon, Tue, etc.)
      formatString = props.withTime ? "ddd-MM-YYYY HH:mm:ss" : "ddd-MM-YYYY";
    }
  }
  
  // Set time components if enabled
  if (props.withTime && selectedDate.value) {
    const newDate = new Date(selectedDate.value);
    newDate.setHours(
      selectedHour.value,
      selectedMinute.value,
      selectedSecond.value,
      0
    );
    selectedDate.value = newDate;
  }
  
  // Format the date using moment
  try {
    return moment(selectedDate.value).format(formatString);
  } catch (e) {
    console.error("Error formatting date:", e);
    return selectedDate.value ? selectedDate.value.toLocaleDateString() : '';
  }
});