<script setup>
  import { computed } from 'vue';

  // Переменная даты
  const today = new Date()
  
  // Емит для слушателя handleDateSelection в App.vue
  const emit = defineEmits(['date-selected']);

  //Пропс
  const props = defineProps({ 
    day: Number,
    month: Number,
    year: Number, 
    selectedFullDate: Object,
    complete: Object
  })

  // Определение дней в месяце
  const daysInMonth = computed(() => { 
    return new Date(props.year, props.month + 1, 0).getDate();
  })

  // С какого дня пн, вт и тд начинается неделя
  const startDay = computed(() => {
    let day = new Date(props.year, props.month, 1).getDay();
    return (day === 0) ? 6 : day - 1;
  })

  // Массив дат в месяце
  const calendar = computed(() => { 
    const days = [];
    for (let i = 0; i < startDay.value; i++) { days.push(null) };
    for (let i = 1; i <= daysInMonth.value; i++) { days.push(i) };
    return days;
  })

  //для emit в слушатель App.vue
  function onDayClick(day) { 
    const select = { 
      day, 
      month: props.month,
      year: props.year,
    }
    emit('date-selected', select)
  }
  
  // Проверка на дату которую нажали для класса selected 
  function isSelected(day) { 
  if (!props.selectedFullDate) return false;
  return props.selectedFullDate.day === day &&
         props.selectedFullDate.month === props.month &&
         props.selectedFullDate.year === props.year;
  }

  // Для класса CSS проверка на сегодняшнее число
  function isToday(day) { 
    return today.getDate() === day && 
           today.getMonth() === props.month && 
           today.getFullYear() === props.year;
  }

  // Для пометки цветом в зависимости от выполненых заметок
  function getNoteStatusForDay(day) { 
    const dateKey = `${props.year}-${props.month}-${day}`;
    return props.complete[dateKey] || null
  }

</script>

<template>
  <div class="Calendar">
      <div class="header-calendar">
          <div class="days-in-week">Пн</div>
          <div class="days-in-week">Вт</div>
          <div class="days-in-week">Ср</div>
          <div class="days-in-week">Чт</div>
          <div class="days-in-week">Пт</div>
          <div class="days-in-week">Сб</div>
          <div class="days-in-week">Вс</div>
      </div>
      <div class="body-calendar">
        <div v-for="(day, index) in calendar" 
        :key="index"
        :class="{
          countWeek: true,
          }"
          >
            <span 
            v-if="day !== null"
            @click="onDayClick(day)"
            :class="{
              selected: isSelected(day),
              today: isToday(day),
              day: true,
               'has-note': getNoteStatusForDay(day),
               'all-complete': getNoteStatusForDay(day)?.allCompleted 
            }"
            > {{ day }} </span>
            <span v-else>&nbsp;</span>
        </div>
      </div>
  </div>
</template>

<style scoped>

  .Calendar {
    width: 100%;
    max-width: 600px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    text-align: center;
  }

  .header-calendar {
    display: grid; 
    grid-template-columns: repeat(7, 1fr); 
    padding-bottom: 40px;
  }

  .days-in-week{
    font-size: 20px;
    font-weight: 700;
    text-align: center;
  }

  .body-calendar {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    row-gap: 50px;
  }

  .countWeek {
    text-align: center;
    font-size: 25px;
  }

  .day { 
    font-size: 20px;
    cursor: pointer;
  }

  .day.has-note { 
    background-color: #dc3545;
    padding: 7px;
    border-radius: 10px;
    color: white;
  }

  .day.all-complete{ 
    background-color: #218838;
    padding: 7px;
    border-radius: 10px;
    color: white;
  }

  .today{ 
    padding: 7px;
    border-radius: 10px;
    background-color: black;
    color: white;
  }

  .selected { 
    padding: 7px;
    border-radius: 10px;
    background-color: black;
    color: white;
  }

</style>
