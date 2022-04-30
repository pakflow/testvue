<script>
import axios from "axios";
export default {
  data() {
    return {
      page: 0,
      users: [],
      count: 5
    };
  },

  methods: {
    increase() { 
      this.page++;
    },
    decrease() {
      this.page--;
    },
    getUsers() {
      axios
        .get(`https://test-js.alef.dev/users?page=${this.page}&count=${this.count}`)
        .then((data) => {
          this.users = data.data.data;
        });
    },
    getUser() {
      axios
        .get(`https://test-js.alef.dev/user/${this.key}`)
        .then((data) => {
          this.user = data.data.id;
        });
        console.log(data.data.id);
    }
  },
  watch: {
    users() {
      this.getUsers();
    },
  },
  mounted() {
    this.getUsers();
  },
};
</script>

<template>
  <main>
    <div>
      <div v-for="user in users" :key="user.id" @click="getUser">
        {{ user.name }}
      </div>
    </div>
    <button v-if="page > 0" @click="decrease">назад</button>
    {{ page }}
    <button v-if="users.length === count" @click="increase">вперед</button>
  </main>
</template>