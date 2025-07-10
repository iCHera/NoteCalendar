<template>
  <div class="Calendar">
    <table class="table">
      <thead>
        <tr>
          <th class="day-week">Пн</th>
          <th class="day-week">Вт</th>
          <th class="day-week">Ср</th>
          <th class="day-week">Чт</th>
          <th class="day-week">Пт</th>
          <th class="day-week">Сб</th>
          <th class="day-week">Вс</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="week in weeks" :key="week">
          <td v-for="day in week" :key="day" class="count-day">
            <span 
            v-if="day"
            @click="onDayClick(day)"
            :class="{
              selected: isSelected(day),
              today: isToday(day),
              day: true,
            }"
            > {{ day }} </span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
  import { computed } from 'vue';

  const today = new Date()
  const emit = defineEmits(['date-selected']);

  const props = defineProps({ 
    day: Number,
    month: Number,
    year: Number, 
    selectedFullDate: Object,
  })

  const daysInMonth = computed(() => { 
    return new Date(props.year, props.month + 1, 0).getDate();
  })

  const startDay = computed(() => {
    let day = new Date(props.year, props.month, 1).getDay();
    return (day === 0) ? 6 : day - 1;
  })

  const calendar = computed(() => { 
    const days = [];
    for (let i = 0; i < startDay.value; i++) { days.push(null) };
    for (let i = 1; i <= daysInMonth.value; i++) { days.push(i) };
    return days;
  })

  const weeks = computed(() => { 
    const week = []
    const w = [...calendar.value]
    for (let i = 0; i < w.length; i += 7) { week.push(w.slice(i, i + 7)) }
    return week;
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
  
  //Проверка на дату которую нажали для класса selected 
  function isSelected(day) { 
  if (!props.selectedFullDate) return false;
  return props.selectedFullDate.day === day &&
         props.selectedFullDate.month === props.month &&
         props.selectedFullDate.year === props.year;
  }

  function isToday(day) { 
    return today.getDate() === day && 
           today.getMonth() === props.month && 
           today.getFullYear() === props.year;
  }


</script>

<style>

  .Calendar {
    display: flex;
    flex-direction: column;
    max-width: 1000px;
    max-height: 1000px;
  }

  .Month {
    display: flex;
    justify-content: flex-end;
    padding: 0 30px 0 0;
    margin: 0;
  }

  .table { 
    width: 600px;
    height: 600px;
  }

  .day-week {
    font-size: 20px;
  }

  .count-day {
    text-align: center;
    vertical-align: middle;
  }

  .today {
    background-color: red;
    color: white;
    border-radius: 10px;
  }

  .day { 
    display: inline-flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    cursor: pointer;
    width: 35px;
    height: 35px;
  }

  .selected{ 
    background-color: black;
    color: white;
    border-radius: 10px;
  }

</style>
