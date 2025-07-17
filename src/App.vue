<script setup>
  import { ref, computed, watch} from 'vue';
  import Calendar from './components/Calendar.vue';
  import SliderMonth from './components/SliderMonth.vue';
  import NoteItems from './components/NoteItems.vue';
import { jsx } from 'vue/jsx-runtime';

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
  const storageKey = 'vue-calendar-notes';
  const allNote = ref(JSON.parse(localStorage.getItem(storageKey)) || {})

  // Наблюдатель за allNote
  watch(allNote, (newNote) => { 
    const jsonNotes = JSON.stringify(newNote)
    localStorage.setItem(storageKey ,jsonNotes)
  }, {deep: true}) 

  // Для изменения цвета в зависимости от того выполенные задаения или нет
  const noteStatusMap = computed(() => { 
    const statusMap = {}

    for(const dateKey in allNote.value) { 
      const notesOnDate = allNote.value[dateKey]

      if (notesOnDate && notesOnDate.length > 0) { 
        const allCompleted = notesOnDate.every(note => note.complete === 'isTrue');
        statusMap[dateKey] = { 
          hasNote: true,
          allCompleted: allCompleted,
        }
      }
    }
    return statusMap
  })

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
      complete: 'isFasle',
    }

    allNote.value[dateKey].push(newNote)
  }

  // Удаление заметки
  function deleteNote(noteId) { 
    if(!selectedDate.value) return;
    const dateKey = `${selectedDate.value.year}-${selectedDate.value.month}-${selectedDate.value.day}`;
    allNote.value[dateKey] = allNote.value[dateKey].filter(note => note.id !== noteId)
  }

  // Изменение complete на выполнено
  function completeNote(noteId) { 
  const dateKey = `${selectedDate.value.year}-${selectedDate.value.month}-${selectedDate.value.day}`;
  const currentNotes = allNote.value[dateKey];
  const updatedNotes = currentNotes.map((note) => {
    if (note.id === noteId) return { ...note, complete: 'isTrue' };
    else return note;
  });
  allNote.value[dateKey] = updatedNotes;
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
     :complete="noteStatusMap"
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
      @complete-note="completeNote"
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
