<script setup>
import { ref, onMounted } from 'vue'


const msg = ref('')
const count = ref(0)
const showNum = ref(true)

function inc(val = 1) {
  count.value = count.value +  val
}

function hideNum() {
  showNum.value = !showNum.value
}


const users = ref([])
const loading = ref(true)
const error = ref(null)

onMounted(async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/users')
    const data = await res.json()
    users.value = data
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
})
</script>

<template>
  <h1>Hello {{ msg }}</h1>
  <input v-model="msg" placeholder="Type Name" />
  <h2 v-if="showNum">{{count}}</h2>
  <button @click="inc()">Inc by 1</button>
  <button @click="inc(3)">Inc By 3</button>
  <button @click="hideNum()">{{showNum ? 'Hide' : 'Show'}} Number</button>

<div>
    <h3>Users List:</h3>
    <div v-if="loading">Loading...</div>
    <div v-else-if="error">Error: {{ error }}</div>
    <ul v-else>
      <li v-for="user in users" :key="user.id">{{ user.name }}</li>
    </ul>
  </div>

</template>
