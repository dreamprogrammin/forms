<script setup>
import {reactive, computed} from 'vue'
import {email, maxLength, minLength, required, requiredPhone, sameAs} from "./utils/i18n-validators.js";
import { vMaska } from 'maska/vue';
import useVuelidate from '@vuelidate/core';
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
    requiredPhone: requiredPhone(24)
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

const v = useVuelidate(rules, form)

const validate = ({prop}) => {
  const error = v.value.$errors.find((el) => el.$property === prop)

  return error && error.$message
}

const resetForm = () => {
  Object.keys(form).forEach((key) => {
    form[key] = null
  })

  v.value.$reset()
}


const onSubmit = async () => {
  v.value.$touch

  if (form.pending) return

  if (await v.value.$validate()) {
    form.pending = true

    try {
      const payload = {
      name: form.name,
      email: form.email,
      phone: form.phone,
      message: form.message,
      password: form.password,
      currentPassword: form.currentPassword,
      checkbox: form.checkbox
    }
    setTimeout(() => {
    console.log('запрос отправлен')
    console.log(payload)

    resetForm()
    },2500)
    } catch (e) {
      console.error(e)
    }
  }
  console.log('123a')
}
</script>
<script>
export default {
   directives : {
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
        <input 
          class="input" 
          @focus="v.$reset()"
          :class="{'is-danger': !!validate({prop: 'name'})}" 
          type="text" 
          v-model="form.name">
      </div>
      <p 
        v-if="!!validate({prop : 'name'})"
        class="help is-danger"
      >
        {{ validate({prop: 'name'}) }}
      </p>
    </div>
    <div class="field">
      <label class="label">email</label>
      <div class="control">
        <input 
          class="input"
          @focus="v.$reset()"
          :class="{'is-danger': !!validate({prop: 'email'})}" 
          type="email"
          v-model="form.email">
      </div> 
      <p 
        v-if="!!validate({prop : 'email'})"
        class="help is-danger"
      >
        {{ validate({prop: 'email'}) }}
      </p>
    </div>
    <div class="field">
      <label class="label">Номер телефона</label>
      <div class="control">
        <input 
          class="input"
          @focus="v.$reset()"
          :class="{'is-danger': !!validate({prop: 'phone'})}"  
          type="text" 
          v-maska
          data-maska="+7 (###) - ### - ## - ##"
          v-model="form.phone">
      </div>
      <p 
        v-if="!!validate({prop : 'phone'})"
        class="help is-danger"
      >
        {{ validate({prop: 'phone'}) }}
      </p>
    </div>
    <div class="field">
      <label class="label">Сообщение</label>
      <div class="control">
        <textarea 
          @focus="v.$reset()"
          :class="{'is-danger': !!validate({prop: 'message'})}"  
          class="textarea" 
          v-model="form.message">
        </textarea>
      </div>
      <p 
        v-if="!!validate({prop : 'message'})"
        class="help is-danger"
      >
        {{ validate({prop: 'message'}) }}
      </p>
    </div>
    <div class="field">
      <label class="label">Пароль</label>
      <div class="control">
        <input 
          class="input"
          @focus="v.$reset()"
          :class="{'is-danger': !!validate({prop: 'password'})}"   
          type="password" 
          v-model="form.password">
      </div>
      <p 
        v-if="!!validate({prop : 'password'})"
        class="help is-danger"
      >
        {{ validate({prop: 'password'}) }}
      </p>
    </div>
    <div class="field">
      <label class="label">Повторите пароль</label>
      <div class="control">
        <input
          @focus="v.$reset()"
          :class="{'is-danger': !!validate({prop: 'currentPassword'})}"   
          class="input" 
          type="password" 
          v-model="form.currentPassword">
      </div>
      <p 
        v-if="!!validate({prop : 'currentPassword'})"
        class="help is-danger"
      >
        {{ validate({prop: 'currentPassword'}) }}
      </p>
    </div>
    <div class="field">
      <label 
        class="checkbox"
        :class="{'has-text-danger': !!validate({prop: 'checkbox'})}"  
        >
        <input type="checkbox" v-model="form.checkbox" @focus="v.$reset()">
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
