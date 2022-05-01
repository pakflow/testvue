<script>
import axios from "axios";
export default {
  props: ["roles", "userRoles", "userId"],
  data() {
    return {
      allRoles: this.roles,
      notSelectedRoles: this.roles
        .map((_, n) => {
          if (!this.userRoles.includes(n)) {
            return n;
          }
        })
        .filter((el) => !!el | (el === 0)),
      selectedRoles: this.userRoles || [],
    };
  },
  methods: {
    addRole(index) {
      axios
        .post(`https://test-js.alef.dev/user/${this.userId}/role/${index + 1}`)
        .then((response) => {
          if (response.status === 200) {
            this.selectedRoles.push(index);
            this.notSelectedRoles = this.notSelectedRoles.filter(
              (notSelectedIndex) => notSelectedIndex !== index
            );
          }
        });
    },
    deleteRole(index) {
      axios
        .delete(
          `https://test-js.alef.dev/user/${this.userId}/role/${index + 1}`
        )
        .then((response) => {
          if (response.status === 200) {
            this.selectedRoles = this.selectedRoles.filter(
              (selectedRolesIndex) => selectedRolesIndex !== index
            );
            this.notSelectedRoles.push(index);
          }
        });
    },
  },
};
</script>

<template>
<div>
  <select multiple="multiple">
    <option
      v-for="index in notSelectedRoles"
      :key="index"
      @click="addRole(index)"
    >
      {{ allRoles[index] }}
    </option>
  </select>
  <select multiple="multiple">
    <option v-for="index in selectedRoles" :key="index" @click="deleteRole(index)">
        {{ allRoles[index] }}
    </option>
  </select>
  </div>
</template>

<style>
  select {
      min-width: 200px;
  }
</style>