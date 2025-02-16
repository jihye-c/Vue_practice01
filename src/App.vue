<script setup>
import { ref, reactive, computed, watch } from 'vue'
const names = reactive(['홍 길동', '김 영희', '이 철수', '박 아무개'])
const selected = ref('')
const prefix = ref('')
const first = ref('')
const last = ref('')

const filterdNames = computed(() =>
  names.filter((n) => n.toLowerCase().startsWith(prefix.value.toLowerCase())),
)

watch(selected, (name) => {
  ;[last.value, first.value] = name.split(' ')
})

function create() {
  if (hasValidInput()) {
    const fullName = `${last.value} ${first.value}`
    if (!names.includes(fullName)) {
      names.push(fullName)
      first.value = last.value = ''
    }
  }
}

function update() {
  if (hasValidInput() && selected.value) {
    const i = names.indexOf(selected.value)
    names[i] = selected.value = `${last.value} ${first.value}`
  }
}

function del() {
  if (selected.value) {
    const i = names.indexOf(selected.value)
    names.splice(i, 1)
    selected.value = first.value = last.value = ''
  }
}

function hasValidInput() {
  return first.value.trim() && last.value.trim()
}
</script>

<template>
  <v-container class="pa-8">
    <v-row>
      <v-text-field variant="underlined" label="Filterd Prefix" class="w-100" v-model="prefix">
      </v-text-field>
      <select select size="10" class="my-4 w-100" v-model="selected">
        <option v-for="name in filterdNames" :key="name">{{ name }}</option>
      </select>
    </v-row>
    <v-row class="d-flex ga-3 mt-8">
      <v-text-field variant="outlined" label="surname" v-model="last"></v-text-field>
      <v-text-field variant="outlined" label="name" v-model="first"></v-text-field>
    </v-row>
    <v-row class="ga-2 d-flex justify-center">
      <v-btn variant="outlined" @click="create">Create</v-btn>
      <v-btn variant="outlined" @click="update">Update</v-btn>
      <v-btn variant="outlined" @click="del">Delete</v-btn>
    </v-row>
  </v-container>
</template>

<style module>
select {
  border: 1px solid black;
  padding: 12px;
}
</style>
