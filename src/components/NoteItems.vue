<script setup>
    import { computed, ref } from 'vue'
    
    const props = defineProps({ 
        selectDate: Object,
        notes: Array,
    })

    const emit = defineEmits([
        'add-note',
        'delete-note',
        'close-note', 
        'complete-note'
    ])

    const noteText = ref('')
    const dayNow = new Date()
    const selectDateCheck = computed(() => { 
        return new Date(props.selectDate.year, props.selectDate.month, props.selectDate.day)
    })

    function handlerAddnote() { 
        if(noteText.value.trim()) {
            emit('add-note', noteText.value)
            noteText.value = ''
        }
    }

    function handlerDeleteNote(noteId) { 
        emit('delete-note', noteId)
    }

    function handlerCompleteNote(noteId) { 
        emit('complete-note', noteId)
    }

    function closeNote() { 
        emit('close-note')
    }

</script>

<template>
     <div class="note-container">
        <div class="header-note-container">
            <h1 class="note-date"> Выбрана дата: {{ props.selectDate.day }}.{{ props.selectDate.month + 1 }}.{{ props.selectDate.year }} </h1>
            <button class="close-note-container-button" @click="closeNote"> X </button>
        </div>
        <div class="note-item" v-if="dayNow <= selectDateCheck  || 
        (dayNow.getDate() === selectDateCheck.getDate() && dayNow.getMonth() === selectDateCheck.getMonth() && dayNow.getFullYear() === selectDateCheck.getFullYear())">
            <input 
            type="text"
            placeholder="Введите новую заметку..."
            class="note-add-input"
            v-model="noteText"
            @keyup.enter="handlerAddnote">

            <button class="note-add-button"
            :class="{'is-active': noteText.trim() !== ''}"
            @click="handlerAddnote">Добавить</button>
        </div>

        <ol v-if="props.notes.length > 0" class="ol-list">
            <li v-for="(note, index) in props.notes" :key="note.id" class="li-list">
                <div class="div-li-list" :class="{'is-completed': note.complete === 'isTrue'}">
                    <span class="note-text"> {{ index + 1 }} {{ note.text }} </span>
                    <div class="note-list-buton">
                        <button @click="handlerCompleteNote(note.id)" v-if="note.complete !== 'isTrue'" class="completed-button">✓</button>
                        <button @click="handlerDeleteNote(note.id)" class="delete-button"> X </button>
                    </div>
                </div>
            </li>
        </ol>
     </div>
</template>

<style scoped>

    .note-container{ 
        margin-top: 20px;
        padding: 20px;
        border: 1px solid;
        border-radius: 20px;
    }

    .header-note-container{ 
        display: flex;
        justify-content: space-between;
        align-items: center;
        text-align: center;
        padding: 0 0 20px 0;
    }

    .close-note-container-button{ 
        cursor: pointer;
        color: white;
        background-color: black;
        border-radius: 7px;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0; 
        padding: 2px 7px;
        transition: 0.7s;
    }

    .close-note-container-button:hover { 
        background-color: #dc3545;
    }

    .note-date { 
        margin: 0;
        text-align: center;
        font-size: 20px;
    }

    .note-item{ 
        display: flex;
        gap: 10px;
    }

    .note-add-input{ 
        width: 100%;
        border-radius: 7px;
        padding-left: 10px;
    }

    .note-add-button{
        background-color: black;
        border: none;
        border-radius: 7px;
        color: white;
        font-weight: 800;
        font-size: 15px;
        padding: 10px;
        cursor: not-allowed;
        transition: 0.7s ease;
    }

    .note-add-button.is-active{ 
        background-color: #218838;
        cursor: pointer;
    }

    .ol-list {
        margin: 0;
        padding: 10px 0 0 0;
    }

    .li-list {
        padding-top: 10px;
        list-style: none;
    }

    .div-li-list{ 
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        gap: 20px;
        background-color: rgba(220, 53, 69, 0.2);
        border-radius: 7px;
        padding: 5px 2px;
        transition: 0.7s ease;
    }

    .div-li-list.is-completed { 
        background-color: rgba(33, 136, 56, 0.2);
    }

    .note-text {
        font-size: 17px;
        flex-grow: 1; 
        text-align: left; 
        word-break: break-word;
        padding-left: 10px;
    }

    .note-list-buton{ 
        display: flex;
        gap: 10px;
    }

    .completed-button{ 
        cursor: pointer;
        color: white;
        background-color: black;
        border-radius: 7px;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0; 
        padding: 0px 6px;
        transition: 0.7s ease;
    }

    .completed-button:hover { 
        background-color: #218838;
    }

    .delete-button {
        cursor: pointer;
        color: white;
        background-color: black;
        border-radius: 7px;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0; 
        padding: 2px 7px;
        transition: 0.7s;
    }

    .delete-button:hover {
        background-color: #dc3545;
    }
</style>