<template>
  <header class="header-global">
    <base-nav class="navbar-main" transparent type="" effect="light" expand>
      <router-link v-if="!user" to="/" slot="brand" class="navbar-brand mr-lg-5">
        <img src="img/brand/white.png" />
      </router-link>
      <router-link v-else to="/landing" slot="brand" class="navbar-brand mr-lg-5">
        <img src="img/brand/white.png" />
      </router-link>

      <div class="row" slot="content-header">
        <div class="col-6 collapse-brand">
          <a href="/">
            <img src="img/brand/blue.png" />
          </a>
        </div>
      </div>

      <ul v-if="user" class="navbar-nav navbar-nav-hover align-items-lg-center">
        <router-link to="/movies" class="nav-link">Movies</router-link>
        <router-link to="/favorites" class="nav-link">Favorites</router-link>
      </ul>
      <ul v-if="user" class="navbar-nav align-items-lg-center ml-lg-auto">
        <li class="nav-item d-none d-lg-block ml-lg-4">
          {{ user.name }}
        </li>
        <li class="nav-item d-none d-lg-block ml-lg-4">
          <a href="#" @click="logout($event)" class="nav-link">Logout</a>
        </li>
      </ul>
      <ul v-else class="navbar-nav align-items-lg-center ml-lg-auto">
        <router-link to="/register" class="nav-link">Register</router-link>
        <li class="nav-item d-none d-lg-block ml-lg-4">
          <router-link to="/login" class="nav-link">Login</router-link>
        </li>
      </ul>
    </base-nav>
  </header>
</template>
<script>
import BaseNav from "@/components/BaseNav";
import BaseDropdown from "@/components/BaseDropdown";
import Cookies from "../utils/Cookies";
import jwtDecode from "jwt-decode";
import router from "@/router";

export default {
  components: {
    BaseNav,
    BaseDropdown
  },
  computed: {
    user: function() {
      return this.$store.state.user;
    }
  },
  created() {
    const token = Cookies.read("token");

    if (token) {
      const user = jwtDecode(token);
      this.$store.dispatch("LOGIN", user);
    }
  },
  methods: {
    logout(e) {
      e.preventDefault();

      /* First we remove the token in order to stop logging the user on app start-up */
      console.log(Cookies.remove("token"));
      router.push("/");
      /* Then we clear the user object from our store */
      return this.$store.dispatch("LOGOUT");
    }
  }
};
</script>
<style></style>
