<script setup>
import {reactive, computed} from 'vue'
import {email, maxLength, minLength, required, requiredPhone, sameAs} from "./utils/i18n-validators.js";
import { vMaska } from 'maska/vue';
const form = reactive({
  name: null,
  email: null,
  phone: null,
  message: null,
  password: null,
  currentPassword: null,
  checkbox: null,
  pending: null
})
const rules = computed(() => ({
  name: {
    required,
  },
  email: {
    required,
    email
  },
  phone : {
    required,
    requiredPhone: requiredPhone(18)
  },
  message : {
    required,
    minLength: minLength(10),
  },
  password: {
    required,
    minLength: minLength(8),
    maxLength: maxLength(15)
  },
  currentPassword: {
    required,
    minLength: minLength(8),
    maxLength: maxLength(15),
    sameAs: sameAs(form.password)
  },
  checkbox: {
    required,
    sameAs: sameAs(true)
  }
}))

const onSubmit = () => console.log('hi')
</script>
<script>
export default {
   directive : {
    maska: vMaska
   }
}
</script>

<template>
  <div class="container">
    <div class="field">
      <label class="label">
        Имя
      </label>
      <div class="control">
        <input class="input" type="text" v-model="form.name">
      </div>
    </div>
    <div class="field">
      <label class="label">email</label>
      <div class="control">
        <input class="input" type="email" v-model="form.email">
      </div> 
    </div>
    <div class="field">
      <label class="label">Номер телефона</label>
      <div class="control">
        <input class="input" type="text" v-maska data-maska="+7 (###) - ### - ## - ##" v-model="form.phone">
      </div>
    </div>
    <div class="field">
      <label class="label">Сообщение</label>
      <div class="control">
        <textarea class="textarea" v-model="form.message"></textarea>
      </div>
    </div>
    <div class="field">
      <label class="label">Пароль</label>
      <div class="control">
        <input class="input" type="password" v-model="form.password">
      </div>
    </div>
    <div class="field">
      <label class="label">Повторите пароль</label>
      <div class="control">
        <input class="input" type="password" v-model="form.currentPassword">
      </div>
    </div>
    <div class="field">
      <label class="checkbox">
        <input type="checkbox" v-model="form.checkbox">
          я соглашаюсь с <a href="#">условие сайта</a>
      </label>
    </div>

    <div class="control mx-auto mt-2">
      <button @click="onSubmit" :disabled="form.pending" :class="{'is-loading' : form.pending}" class="button is-link">Отправить</button>
    </div>
  </div>
</template>

<style scoped>
</style>
