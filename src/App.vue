<template>
  <div id="nav">
    | <router-link to="/">Home</router-link> |
    <router-link to="/orders">Orders</router-link> |
    <router-link to="/add-driver">Add Driver</router-link> |
    <router-link to="/add-truck">Add Truck</router-link> |
    <router-link to="/login">Login</router-link> |
  </div>
  <router-view />
</template>

<script>
// import { mapGetters } from 'vuex';
// import axios from 'axios';

export default {
  name: "App",
  beforeCreate() {
    this.$store.commit("auth/initializeStore");

    // const token = this.$store.state.token

    // if (token) {
    //   axios.defaults.headers.post['Authorization'] = 'Bearer ' + token
    //   // axios.defaults.headers.post['Content-Type'] = 'application/json'
    // } else {
    //   axios.defaults.headers.common.post['Authorization'] = ""

    // }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0;
  background: #eee;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #b9a13f;
}
</style>
