<template>
  <!-- declare bind with : -->
  <div
    class="container"
    :class="{ 'light-background': !isDarkMode, 'dark-backgrond': isDarkMode }"
  >
    <Notification v-if="hasText" v-bind:text="text" />
    <RequestAccount />
    <div class="login">
      <!-- if isDarkMode is true, apply images for darkmode -->
      <img src="@/assets/DCHQ.svg" v-if="isDarkMode" />
      <img src="@/assets/DCHQ-dark.svg" v-if="!isDarkMode" />
      <h4 :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }">
        Sign into Design+Code HQ
      </h4>
      <!-- prevent is instead of e.preventdefault() -->
      <form @submit.prevent="onSubmit">
        <input
          :class="{ 'light-field': isDarkMode, 'dark-field': !isDarkMode }"
          type="email"
          placeholder="Email"
          v-model="email"
          required
        />
        <input
          :class="{ 'light-field': isDarkMode, 'dark-field': !isDarkMode }"
          type="password"
          placeholder="Password"
          v-model="password"
        />
        <button>Sign In</button>
      </form>
      <router-link
        :class="{ 'light-text': isDarkMode, 'dark-text': !isDarkMode }"
        to="/recover"
      >
        Forgot your password?
      </router-link>
      <ThemeSwitch />
    </div>
  </div>
</template>
<script>
import { Component, Vue } from "vue-property-decorator";
import Header from "@/components/Header.vue";
import RequestAccount from "@/components/RequestAccount.vue";
import Notification from "@/components/Notification.vue";
import ThemeSwitch from "@/components/ThemeSwitch.vue";
import { auth } from "@/main";

export default {
  name: "SignIn",
  components: {
    RequestAccount,
    ThemeSwitch,
    Notification
  },
  data() {
    return {
      // null = no value
      email: null,
      password: null,
      hasText: false,
      text: ""
    };
  },
  // init state
  computed: {
    isDarkMode() {
      return this.$store.getters.isDarkMode;
    }
  },
  methods: {
    onSubmit() {
      // "this" here is specifying v-model above
      const email = this.email;
      const password = this.password;
      // add parameter from the element which defined as v-model above

      auth
        .login(email, password, true)
        .then(response => {
          // alert("Response: " + response); // object object
          // $router below is used in main.js
          this.$router.replace("/");
          alert("successfully logged in!");
        })
        .catch(error => {
          alert("Error: " + error);
        });
    }
  },
  mounted() {
    const params = this.$route.params;
    if (params.userLoggedOut) {
      this.hasText = true;
      this.text = "You have successfully logged out.";
    } else if (params.userRecoveredAccount) {
      this.hasText = true;
      this.text = `A recovery email has been sent to ${params.email}`;
    } else if (params.userRequestedAccount) {
			this.hasText = true;
			this.text = `Your request has been sent for ${params.email}`;
		}
  }
};
</script>
