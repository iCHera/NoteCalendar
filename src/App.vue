<script setup>
  import { ref, computed } from 'vue';
  import Calendar from './components/Calendar.vue';
  import SliderMonth from './components/SliderMonth.vue';
  import NoteItems from './components/NoteItems.vue';

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

  // Все заметки что есть
  const allNote = ref({})

  // Определение даты для 
  const notesForSelectedDate = computed(() => { 
    if(!selectedDate.value) return [];
    const dateKey = `${selectedDate.value.year}-${selectedDate.value.month}-${selectedDate.value.day}`;
    return allNote.value[dateKey] || []
  })

  // Добавление заметки
  function addNote(noteText) { 
    if(!selectedDate.value) return;
    const dateKey = `${selectedDate.value.year}-${selectedDate.value.month}-${selectedDate.value.day}`;
    
    if (!allNote.value[dateKey]) { 
      allNote.value[dateKey] = []
    }

    const newNote = { 
      id: new Date(),
      text: noteText,
    }

    allNote.value[dateKey].push(newNote)
  }

  // Удаление заметки
  function deleteNote(noteId) { 
    if(!selectedDate.value) return;
    const dateKey = `${selectedDate.value.year}-${selectedDate.value.month}-${selectedDate.value.day}`;
    allNote.value[dateKey] = allNote.value[dateKey].filter(note => note.id !== noteId)
  }

  // Закрытие заметок
  function closeNote() { 
    selectedDate.value = null
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

    <div v-if="selectedDate !== null">
      <NoteItems
      :select-date="selectedDate"
      :notes="notesForSelectedDate"  
      @close-note="closeNote"
      @add-note="addNote"
      @delete-note="deleteNote"
      />
    </div>
  </div>
</template>

<style scoped>

  .app-container{ 
    max-width: 600px;
    margin: 0 auto;
  }

  .calendar { 
    display: flex;
    justify-content: center;
  }

</style>
