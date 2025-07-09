<template>
  <div class="Calendar">
    <h1 class="Month">{{ Month }}</h1>
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
            @click="selectDay(day)"
            :class="{
              selected: day === selectDate, 
              today: day === dayNow,
              day
            }"
            > {{ day }} </span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
  import { computed, ref } from 'vue' 
  
  const dayNow = ref(new Date().getDate());
  const currentMonth = ref(new Date().getMonth());
  const currentYears = ref(new Date().getFullYear());

  // Определение дней в месяце 
  const daysInMonth = computed(() => { 
    return new Date(currentYears.value, currentMonth.value + 1, 0).getDate();
  })

  // Определение первого дня в месяце
  const startDay = computed(() => { 
    let day = new Date(currentYears.value, currentMonth.value, 1).getDay()
    return (day + 6) % 7    
  })

  // Заполнение массива датами
  const calendar = computed(() => {
    const days = []
    for(let i = 0; i < startDay.value; i++) { 
      days.push(null)
    }
    for(let i = 1; i < daysInMonth.value; i++) { 
      days.push(i);
    }
    return days
  })

  // Из массива разбиваю на недели
  const weeks = computed(() => {
    const week = [] 
    for (let i = 0; i <= calendar.value.length; i += 7) { 
      week.push(calendar.value.slice(i, i + 7));
    }
    return week
  })

    // Определения что за месяц сейчас
  const arrNameMonth = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
  const Month = computed(() => arrNameMonth[currentMonth.value])

  // Функция для работы с выбранной датой
  const selectDate = ref({
    day: null, 
    month: null, 
    year: null,
  })  

  function selectDay(day) { 
    selectDate.value = { 
      day, 
      month: currentMonth.value, 
      year: currentYears.value, 
    }
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

</style>
