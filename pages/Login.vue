<template>
  <div class="login-container">
    <p>Sign In</p>
    <form class="login-form" method="post" @submit.prevent="login">
      <div>
        <input type="email" placeholder="Enter your e-mail" v-model="email">
      </div>
      <div>
        <input type="password" placeholder="Enter your password" v-model="password">
      </div>
      <div class="login-button">
        <button type="submit">Sign In</button>
      </div>
    </form>
  </div>
</template>


<script>
  export default {
    name: 'Login',
    data() {
      return {
        email: '',
        password: ''
      };
    },
    methods: {
      async login() {
        try {
          const auth = await this.$axios.$post('/auth/login', {
            email: this.email,
            password: this.password,
          });
          localStorage.setItem("token", auth.token);
          this.$nuxt.$emit('user-logged-in');
          this.$router.push('/projects');
        } catch (e) {
          console.error(e);
        }
      },
    },
  }
</script>


<style lang="scss" scoped>
.login-container {
  width: 500px;
  height: auto;
  margin: 100px auto;
  background-color: #fefefe;
  padding: 55px;
  border-radius: 10px;
  box-shadow: 0 5px 8px #a1a1a1;
  p {
    text-align: center;
    font-size: 24px;
  }
  .login-form {
    div {
      input {
        margin-top: 15px;
        width: 100%;
        height: 60px;
        border: none;
        position: relative;
        padding: 10px;
        top: 10px;
        background-color: #e0e0e0;
        border-radius: 7px;
      }
      input:focus {
        outline: none;
      }
    }
  }
  .login-button {
    display: flex;
    justify-content: center;
    button {
      width: 100%;
      margin-top: 35px;
      text-align: center;
      height: 40px;
      border: none;
      font-size: 16px;
      background-color: #51b057;
      color: #fff;
      cursor: pointer;
      border-radius: 7px;
    }
  }
}
</style>
