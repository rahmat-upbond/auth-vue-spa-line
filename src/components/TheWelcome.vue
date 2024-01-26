<script setup lang="ts">
import WelcomeItem from './WelcomeItem.vue'
import SupportIcon from './icons/IconSupport.vue'
</script>

<template>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>Welcome</template>

    <div v-if="code">
      Code: {{ code }}
      <br>
      <br>
      <button @click="logout()">Logout</button>
    </div>
    <div v-else>
      <button @click="login()">Login</button>
    </div>
  </WelcomeItem>
</template>

<script lang="ts">
import { Auth0Client } from '@auth0/auth0-spa-js'
const URL = 'https://auth-vue-spa-line.vercel.app';

const auth0 = new Auth0Client({
  domain: 'https://be924997.auth.stg.upbond.io',
  clientId: 'pmPLneSgcgilkwrZc3szn',
  authorizationParams: {
    redirect_uri: URL
  }
})

export default {
  data() {
    return {
      code: "",
    };
  },
  mounted() {
    // Accessing all query parameters as an object
    const queryParams = this.$route.query;
    this.code = queryParams.code as string;
  },
  methods: {
    async login() {
      try {
        await auth0.loginWithRedirect()
      } catch (error) {
        console.log(error)
      }
    },
    async logout() {
      try {
        auth0.logout({
          logoutParams: {
            returnTo: URL
          }
        });
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>
<style>
button{ 
  height: 50px;
  width: 100px;
  border-radius: 2rem;
}
</style>