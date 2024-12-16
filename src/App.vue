<script setup>
import {reactive} from 'vue'
const form = reactive({
  name: null,
})
</script>

<template>
  <div class="container">
    <div class="fields">
      <label class="name">Имя</label>
    </div>
    <div class="control">
      <input v-model="form.name" type="text" placeholder="Введите имя" />
    </div>
    {{ form.name }}
  </div>
</template>

<style scoped>
</style>
