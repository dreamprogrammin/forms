<script setup>
import {reactive} from 'vue'
const form = reactive({
  name: null,
  email: null,
  massage: null,
  password: null,
  currentPassword: null,
  checkbox: null
})
</script>

<template>
  <div class="container">
    <div class="fields">
      <label class="label">Имя</label>
    </div>
    <div class="control">
      <input class="input" v-model="form.name" type="text" placeholder="Введите имя" />
    </div>
    <div class="fields">
      <label class="label">Почта</label>
    </div>
    <div class="control">
      <input class="input" v-model="form.email" type="text" placeholder="Введите email" />
    </div>
    <div class="fields">
      <label class="label">Сообщение</label>
    </div>
    <div class="control">
      <textarea class="textarea" v-model="form.massage" type="text" placeholder="Введите сообщение" />
    </div>
    <div class="fields">
      <label class="label">Пароль</label>
    </div>
    <div class="control">
      <input class="input" v-model="form.password" type="password" placeholder="Введите пароль" />
    </div>
    <div class="fields">
      <label class="label">Повторите пароль</label>
    </div>
    <div class="control">
      <input class="input" v-model="form.currentPassword" type="text" placeholder="Повторите пароль" />
    </div>
    <div class="field">
      <div class="control">
        <label class="label">
          <input v-model="form.checkbox" type="checkbox">
          я соглашаюсь<a href="#">правилами сообщества</a>
        </label>
      </div>
    </div>
    <div class="control mt-4 mx-auto">
      <button class="button is-link" @click="onSubmit">
        Отправить
      </button>
    </div>
  </div>
</template>

<style scoped>
</style>
