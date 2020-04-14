<template>
  <div class="admin-sidebar">
    <ul class="admin-menu">
      <li>
        <router-link to="/admin">Dashboard</router-link>
      </li>
      <li>
        <router-link to="/customer">Customer List</router-link>
      </li>
      <li>
        <a v-if="user" @click="logout()">Logout</a>
      </li>
    </ul>
  </div>
</template>

<script>
import EventBus from "../../event";
export default {
  name: "Sidebar",
  props: {
    user: {
      default: function() {
        return null;
      }
    }
  },
  methods: {
    logout() {
      localStorage.removeItem("user");

      EventBus.$emit("userUpdated");
      this.$router.push("Admin");
    }
  }
};
</script>
