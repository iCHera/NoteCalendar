<script setup>
    import { computed } from 'vue';

    // Пропс
    const props = defineProps({ 
        month: Number,
        year: Number,
    })

    // Емит слушатель для handlerNextSliderMonth и handlerBackSliderMonth в App.vue
    const emit = defineEmits([ 
        'next-button',
        'back-button'
    ])

    // Переменные для вывода месяца и года
    const monthNames = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
    const Month = computed(() => monthNames[props.month])
    const Year = computed(() => props.year)

    // Функция для нажатии на кнопку для слушателя
    function nextButton() { 
        emit('next-button')
    }

    // Функция для нажатии на кнопку для слушателя
    function backButton() { 
        emit('back-button')
    }
</script>
     
<template>
    <div class="slider-header">
        <h1 class="Date"> {{ Month }} {{ Year }}</h1>
        <div class="button--div">
            <button
            @click="backButton"
            :class="{
                button: true, 
            }"
            ><-</button>
            <button
            @click="nextButton"
            :class="{
                button: true,
            }"
            >-></button>
        </div>
    </div>
</template>

<style>
    .slider-header{ 
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .button--div{ 
        display: flex;
        gap: 20px;
    }

    .button { 
        cursor: pointer;
        color: white;
        background-color: black;
        border-radius: 7px;
        border: none;
        display: flex;
        align-items: center;
        justify-content: center;
        flex-shrink: 0; 
        padding: 5px 10px;
        transition: 0.7s;
    }

    .button:hover { 
        background-color: #dc3545;
    }
</style>