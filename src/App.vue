<script>
import axios from "axios";
import Multiselect from './components/Multiselect.vue';
export default {
  data() {
    return {
      page: 0,
      users: [],
      count: 5,
      userPage: false,
      user: null,
      userId: null,
      roles: [],
    };
  },
  components: {
    'multi-select': Multiselect,
  },

  methods: {
    increase() {
      this.page++;
    },
    decrease() {
      this.page--;
    },
    returnToList() {
      this.userPage = false;
      this.user = null;
    },
    getUsers() {
      axios
        .get(
          `https://test-js.alef.dev/users?page=${this.page}&count=${this.count}`
        )
        .then((data) => {
          this.users = data.data.data;
        });
    },
    getUser(id) {
      this.userPage = true;
      this.userId = id;
      axios.get(`https://test-js.alef.dev/user/${id}`).then((response) => {
        this.user = response.data.data;
      });
    },
    getRoles() {
      axios.get(`https://test-js.alef.dev/roles`).then((response) => {
        this.roles = response.data.data;
      });
    },
  },
  watch: {
    users() {
      this.getUsers();
    },
  },
  mounted() {
    this.getUsers();
    this.getRoles();
  },
};
</script>

<template>
  <main>
    <div v-if="!userPage">
      <div>
        <div v-for="user in users" :key="user.id" @click="getUser(user.id)">
          {{ user.name }}
        </div>
      </div>
      <button v-if="page > 0" @click="decrease">назад</button>
      {{ page }}
      <button v-if="users.length === count" @click="increase">вперед</button>
    </div>
    <div v-else>
      <button @click="returnToList">вернуться к списку</button>
      <div v-if="user">
        {{ user.name }}
        <br />
        <multi-select :roles="roles" :userRoles="user.roles.map(index => index -1)" :userId="userId"/>
      </div>
    </div>
  </main>
</template>