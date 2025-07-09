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
            <span v-if="day" class="day"> {{ day }} </span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
  import { ref } from 'vue' 

  const date = new Date();

  const dayNow = date.getDate();
  const dayCountNow = date.getDay();
  const monthNow = date.getMonth();
  const yearNow = date.getFullYear();
  const daysInMonth = new Date(yearNow, monthNow + 1, 0);
  const startDay = (new Date(yearNow, monthNow, 1) + 6) % 7

  let Month;
  const arrNameMonth = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
  for (let i = 0; i < arrNameMonth.length; i++) { 
    if (i == monthNow) { 
      Month = arrNameMonth[i];
    }
  }

  const calendar = []
  for(let i = 0; i <= startDay; i++){
    calendar.push(null)
  }
  for (let i = 0; i <= daysInMonth.getDate(); i++) { 
    calendar.push(i);
  }

  const weeks = []
  for (let i = 0; i <= calendar.length; i += 7){
    weeks.push(calendar.slice(i, i + 7))
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

  }

  .day { 
    display: flex;
    justify-content: center;
    font-size: 20px;
    cursor: pointer;
  }

</style>
