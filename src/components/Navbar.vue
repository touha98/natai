<template>
  <b-navbar toggleable="md" class="shadow-sm" type="light" variant="light">
    <b-navbar-brand href="#">Natai</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item to="/">Home</b-nav-item>
        <b-nav-item to="/about">About</b-nav-item>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button
            class="my-2 my-sm-0"
            variant="outline-secondary"
            type="submit"
            >Search</b-button
          >
        </b-nav-form>
        <b-nav-item-dropdown v-if="user" right>
          <!-- Using 'button-content' slot -->
          <template v-slot:button-content>
            <span class="pr-2 pl-3">{{
              detail.displayName.split(" ")[
                detail.displayName.split(" ").length - 1
              ]
            }}</span>
            <img
              v-bind:src="detail.photoURL"
              alt="profile"
              class="rounded-circle"
              style="height: 30px; border:2px solid var(--colorBlue)"
            />
          </template>
          <b-dropdown-item to="/dashboard">Dashboard</b-dropdown-item>
          <b-dropdown-item to="/new-article">Create Article</b-dropdown-item>

          <b-dropdown-item @click="signout">Sign Out</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-button v-if="!user" class="ml-2 px-4 my-2 my-sm-0" to="/login"
          >Login</b-button
        >
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>
<script lang="ts">
import Vue from "vue";
import { auth } from "@/Firebase";

export default Vue.extend({
  data: () => ({}),

  computed: {
    user() {
      return auth.currentUser;
    },
    detail() {
      console.log(this.$store.getters.profile);
      return this.$store.getters.profile;
    }
  },
  methods: {
    async signout() {
      try {
        await this.$store.dispatch("signout");
        this.$router.push("/login");
      } catch (error) {
        alert(error.message);
      }
    }
  }
});
</script>
