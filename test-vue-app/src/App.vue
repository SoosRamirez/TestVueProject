<script setup>
import ModalWindow from "./components/ModalWindow.vue";
import {ref} from "vue";

const show = ref(false)
const initialData = ref({
  field1: 'INI',
  field2: 123,
  field3: true
});


function toggleModal(){
  show.value = !show.value
}

function saveData(formData){
  localStorage.setItem('formData', JSON.stringify(formData));
  alert('Данные сохранены!');
}

function loadData(){
  initialData.value = JSON.parse(localStorage.getItem('formData')) || initialData.value;
}
</script>

<template>
  <div>
    <h1>Тестовое задание на Vue.js</h1>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
    <h2>Модальное окно</h2>
    <button @click="toggleModal">Открыть модальное окно</button>
  </div>
  <ModalWindow
      v-if="show"
      @save="saveData"
      @load="loadData"
      :initial-data="initialData"
      @close="toggleModal"
  />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
