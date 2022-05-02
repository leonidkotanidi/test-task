<template>
  <header>
    <nav>
      <div class="navbar">
        <div class="navbar-logo">
          <nuxt-link to="/">
            <img src="../assets/img/logo.png">
          </nuxt-link>
        </div>
        <div class="navbar-menu">
          <ul>
            <li v-if="!isAuth"><nuxt-link to="/login">LOGIN</nuxt-link></li>
            <li v-if="isAuth"><nuxt-link to="/projects">PROJECTS</nuxt-link></li>
            <li v-if="isAuth" @click="logout()"><nuxt-link to="/login">LOGOUT</nuxt-link></li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
  export default {
    name: 'Navbar',
    data() {
      return {
        isAuth: false,
      }
    },
    created() {
      this.$nuxt.$on('user-logged-in', () => {
        this.isAuth = true;
      })
    },
    mounted() {
      this.isAuth = !!localStorage.getItem('token');
    },
    methods: {
      logout() {
        localStorage.removeItem('token');
        this.isAuth = false;
      }
    }
  }
</script>

<style lang="scss" scoped>
header {
  background-color: #f9f9f9;
}
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  box-shadow: 0px 5px 3px #a1a1a1;
}
.navbar-logo {
  width: 40px;
  height: 40px;
  img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
}
.navbar-menu {
  ul {
    display: flex;
  }
  li {
    list-style: none;
    font-size: 14px;
    padding: 0 15px;
    color: gray;
    a {
      text-decoration: none;
      color: gray;
    }
  }
}
</style>
