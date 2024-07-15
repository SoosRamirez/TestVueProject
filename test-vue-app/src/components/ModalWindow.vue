<script setup>
import {ref, watch} from "vue";

const props = defineProps(['show', 'initialData'])
const emit = defineEmits(['save', 'load', 'close'])

const formData = ref({
  field1: '',
  field2: '',
  field3: false,
});

const errors = ref({
  field1: '',
  field2: '',
})


function validateField1(){
  if (!/^[a-zA-Z]{1,8}$/.test(formData.value.field1)) {
    errors.value.field1 = 'Только буквы (до 8 символов)';
  } else {
    errors.value.field1 = '';
  }
}
function validateField2(){
  if (!/^\d{1,8}(\.\d{1,4})?$/.test(formData.value.field2)) {
    errors.value.field2 = 'Только цифры (до 8 целых и 4 дробных)';
  } else {
    errors.value.field2 = '';
  }
}
function ValidateAndSave(){
  validateField1();
  validateField2();

  if (!errors.value.field1 && !errors.value.field2) {
    emit('save', formData.value);
  }
}

function loadData(){
  emit('load');
  Object.assign(formData.value, props.initialData);
}


</script>

<template>
<div class="modal">
  <div class="modal-content">
    <h3>Заполните форму</h3>
    <label for="field1">Только буквы, не более 8:</label><br>
    <input type="text" id="field1" v-model="formData.field1" maxlength="8"><br>
    <span v-if="errors.field1" class="error">{{errors.field1}}</span>
    <label for="field2">Только цифры (до 8 целых и 4 дробных):</label><br>
    <input type="number" id="field2" v-model="formData.field2" maxlength="12"><br>
    <span v-if="errors.field2" class="error">{{errors.field2}}</span>
    <label for="field3">Чекбокс:</label><br>
    <input id="field3" type="checkbox" v-model="formData.field3"><br><br>

    <button @click="loadData">Загрузить значения</button>
    <button @click="ValidateAndSave">Сохранить значения</button>
    <button @click="$emit('close')">Закрыть модальное окно</button>
  </div>
</div>
</template>

<style scoped>
.modal {
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0,0,0);
  background-color: rgba(0,0,0,0.4);
  padding-top: 60px;
}
.modal-content {
  background-color: rgb(0,0,0);
  margin: 5% auto;
  padding: 20px;
  border: 1px solid #323232;
  width: 80%;
  max-width: 400px;
}
.error {
  color: red;
  font-size: 12px;
}
</style>