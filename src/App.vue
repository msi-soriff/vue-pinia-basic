<script setup>
  import { useUserStore } from './stores/users';
  import { ref } from 'vue';

  const user_store = useUserStore()

  const user_input = ref({
    name: '',
    email: ''
  })

  const sort = ref(false)

  const CreateUser = () => {
    if (
      !user_input.value.name ||
      !user_input.value.email 
    ){
      return alert('plaese enter both name and email')
    }

    user_store.create(user_input.value)

    user_input.value = {
      name: '',
      email: ''
    }
  
  }
  const DeleteUser = id => {
      user_store.delete(id)
      alert("user Deleted with id:" + id)
    }
</script>

<template>
  <main>
    <h1>Team Manager</h1>
    <form @submit.prevent="CreateUser">
      <input 
      type="text"
      placeholder="e.g. Naruto Uzumaki"
      v-model="user_input.name"/>
      <input
      type="email"
      placeholder="e.g. hokage@ninja.com"
      v-model="user_input.email"/>
      <input
      type="submit"
      value="Create user" />

    </form>
    <label> <span>Sort</span><input type="checkbox" v-model="sort"></label>
    
    <div class="users" v-if="!sort">
      <div v-for="user in user_store.users"
      class="user">
        <div>ID: {{ user.id }}</div>
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
        <button @click="DeleteUser(user.id)">Delete</button>
      </div>
    </div>
    <div class="users" v-else>
      <div v-for="user in user_store.usersByName"
      class="user">
      <div>ID: {{ user.id }}</div>
      <h3>{{ user.name }}</h3>
      <p>{{ user.email }}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>

</style>
