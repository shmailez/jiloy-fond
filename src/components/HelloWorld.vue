<!-- <script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="greetings">
    <h1 class="green">{{ msg }}</h1>
    <h3>
      You’ve successfully created a project with
      <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
      <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
    </h3>
  </div>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style> -->

<template>
  <div class="search-component">
    <input
      type="text"
      v-model="searchQuery"
      placeholder="Введите имена пользователей через запятую"
    />
    
    <!-- Отображаем результаты, если они существуют -->
    <ul v-if="results.length">
      
      <li v-for="user in results" :key="user.id">
        <router-link :to="'/' + user.id">
          <!-- <template> -->
            <ImageUserPhoto class="userImage"/>
          <!-- </template> -->
          
        <!-- <img src="" alt=""> -->
        <!-- <p><strong>ID:</strong> {{ user.id }}</p> -->
        
          <p><strong>Имя:</strong> {{ user.name }}</p>
        
        <!-- <p><strong>Username:</strong> {{ user.username }}</p> -->
        <p><strong>Email:</strong> {{ user.email }}</p>
      </router-link>
        <!-- <p><strong>Компания:</strong> {{ user.company.name }}</p> -->
      </li>
    </ul>

    <p v-if="loading">Загрузка...</p>
    <p v-if="!results.length && searchQuery && !loading">Ничего не найдено</p>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import axios from "axios";
import ImageUserPhoto from './image/ImageUserPhoto.vue'

const searchQuery = ref("");
const results = ref([]);
const loading = ref(false);
const error = ref(null);

const fetchUsers = async () => {
  loading.value = true;
  error.value = null;

  try {
    const response = await axios.get('https://jsonplaceholder.typicode.com/users');
    const queries = searchQuery.value
      .split(',')
      .map(query => query.trim().toLowerCase())
      .filter(query => query);

    results.value = response.data.filter(user =>
      queries.some(query => user.name.toLowerCase().includes(query))
    );
  } catch (err) {
    error.value = "Ошибка при загрузке данных";
  } finally {
    loading.value = false;
  }
};

watch(searchQuery, (newQuery) => {
  if (newQuery) {
    fetchUsers();
  } else {
    results.value = [];
  }
});
</script>

<style scoped>
.search-component {
  /* max-width: 400px; */
  /* margin: 20px auto; */
}
input {
  width: 100%;
  padding: 8px;
  /* margin-bottom: 10px; */
  border: 1px solid #ccc;
  border-radius: 4px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  color: #000;
  /* padding: 8px; */
  border-bottom: 1px solid #eee;
  /* display: flex; */
    /* align-items: center; */
    padding: 10px;
    border-radius: 8px;
    background-color: #fff;
    margin-bottom: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    cursor: pointer;
}

.userImage {
  width: 40px;
    height: 40px;
    /* margin-right: 10px;
    border-radius: 50%; */
}
router-link {
  display: flex;
  color: #007bff;
  text-decoration: none;
  cursor: pointer;
}
router-link:hover {
  text-decoration: underline;
}
</style>