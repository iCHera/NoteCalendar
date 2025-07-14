<script setup>
  import { ref, computed } from 'vue';
  import Calendar from './components/Calendar.vue';
  import SliderMonth from './components/sliderMonth.vue';

  // Переменные для работы
  const selectedDate = ref(null)
  const currentDate = ref(new Date())
  const currentDay = computed(() => currentDate.value.getDate())
  const currentMonth = computed(() => currentDate.value.getMonth())
  const currentYear = computed (() => currentDate.value.getFullYear())

  // Слушатель выбранной даты
  function handleDateSelection(dateObject) { 
    selectedDate.value = dateObject
  }

  // Слушатель для слайдера месяца вперед
  function handlerNextSliderMonth() { 
    const nextSlideMonth = new Date(currentDate.value)
    nextSlideMonth.setMonth(nextSlideMonth.getMonth() + 1)
    currentDate.value = nextSlideMonth
  }

  // Слушатель для слайдера месяца назад
  function handlerBackSliderMonth() { 
    const backSlideMonth = new Date(currentDate.value)
    backSlideMonth.setMonth(backSlideMonth.getMonth() - 1)
    currentDate.value = backSlideMonth
  }

</script>

<template>
  <div class="app-container">

    <div class="slider">
      <SliderMonth
      :month="currentMonth"
      :year="currentYear"
      @next-button="handlerNextSliderMonth"
      @back-button="handlerBackSliderMonth"
      />
    </div>

    <div class="calendar">
      <Calendar
     :day="currentDay"
     :month="currentMonth"
     :year="currentYear" 
     :selected-full-date="selectedDate"
     @date-selected="handleDateSelection" 
     />
    </div>

      <div v-if="selectedDate" class="info">
       Выбрана дата: {{ selectedDate.day }}.{{ selectedDate.month + 1 }}.{{ selectedDate.year }}
     </div>
  </div>
</template>

<style>

  .app-container{ 
    max-width: 600px;
    margin: 0 auto;
  }

  .calendar { 
    display: flex;
    justify-content: center;
  }

  .info { 
    padding-top: 50px;
  }
</style>
