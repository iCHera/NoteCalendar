<script setup>
  import { ref, computed } from 'vue';
  import Calendar from './components/Calendar.vue';

  const selectedDate = ref(null)
  const currentDate = ref(new Date())
  const currentDay = computed(() => currentDate.value.getDate())
  const currentMonth = computed(() => currentDate.value.getMonth())
  const currentYear = computed (() => currentDate.value.getFullYear())

  function handleDateSelection(dateObject) { 
    selectedDate.value = dateObject
  }

</script>

<template>
  <div class="app-container">
     <Calendar
     :day="currentDay"
     :month="currentMonth"
     :year="currentYear" 
     :selected-full-date="selectedDate"
     @date-selected="handleDateSelection" />

      <div v-if="selectedDate" class="info">
       Выбрана дата: {{ selectedDate.day }}.{{ selectedDate.month + 1 }}.{{ selectedDate.year }}
     </div>
  </div>
</template>

<style>
  .calendar { 
    display: flex;
    justify-content: center;
  }
</style>
