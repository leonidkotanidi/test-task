<template>
  <div>
    <Navbar />
    <div>
      <Nuxt />
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue"
import Login from "../pages/Login.vue"

export default {
  name: 'default',
  components: {
    Navbar,
    Login
  },
  layout: 'default'
}
</script>
