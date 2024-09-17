<template>
    <div class="user" v-if="user">
      
      <ImageUserPhoto  class="user-img"/>
      <div class="user-info">
        <h2>{{ user.name }} ({{ user.username }})</h2>
        <p><strong>Email:</strong> {{ user.email }}</p>
        <p><strong>Телефон:</strong> {{ user.phone }}</p>
        <p><strong>Компания:</strong> {{ user.company.name }}</p>
        <p><strong>Адрес:</strong> {{ user.address.street }}, {{ user.address.city }}</p>
        <router-link to="/">Назад к поиску</router-link>
      </div>
      
    </div>
  
    <p v-else-if="loading">Загрузка...</p>
    <p v-else-if="error">{{ error }}</p>
  </template>
  
  <script setup>
  import { ref, onMounted } from "vue";
  import { useRoute } from "vue-router";
  import axios from "axios";
  import ImageUserPhoto from "../components/image/ImageUserPhoto.vue"
  
  const route = useRoute();
  const user = ref(null);
  const loading = ref(false);
  const error = ref(null);
  
  const fetchUser = async () => {
    loading.value = true;
    try {
      const response = await axios.get(`https://jsonplaceholder.typicode.com/users?id=${route.params.id}`);
      user.value = response.data[0];
    } catch (err) {
      error.value = "Ошибка при загрузке пользователя";
    } finally {
      loading.value = false;
    }
  };
  
  onMounted(() => {
    fetchUser();
  });
  </script>
  
  <style scoped>

  .user {
    width: 70%;
    /* background-color: rgb(246, 246, 246); */
    color: #000;
    display: flex;
    margin-top: 20px;
    /* display: grid;
    grid-template-columns: 1fr 1fr 1fr; */
    /* display: grid;
    width: 100%; */
  }

  .user-img {
    width: 50%;
    height: 200px;
  }

  .user-info {
    width: 50%;
    height: 200px;
  }

  h2 {
    color: #333;
  }
  a {
    display: inline-block;
    margin-top: 10px;
    color: #007bff;
  }
  a:hover {
    text-decoration: underline;
  }
  </style>