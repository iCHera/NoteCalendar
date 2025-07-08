<template>
  <div>
    <h1 class="day" id="day">Время: {{ hours }}:{{ minute }}:{{ second }}</h1>
  </div>
</template>

<script setup>
import { ref } from "vue";

let hours = ref(new Date().getHours());
let minute = ref(new Date().getMinutes());
let second = ref(new Date().getSeconds())

setInterval( () => { 
 let date = new Date();
 hours.value = date.getHours();
 minute.value = date.getMinutes();
 second.value = date.getSeconds()
}, 1000)

</script>

<style></style>
