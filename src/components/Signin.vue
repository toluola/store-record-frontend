<template>
  <div class="signin-form-container">
    <div class="signin-form">
      <div>
        <h3 class="signin-header">Sign In</h3>
      </div>
      <div>
      <form @submit.prevent="signin">
        <div class="" v-if="error">{{ error }}</div>

        <div>
          <label for="email" class="label">E-mail Address</label><br />
          <input
            type="email"
            v-model="email"
            class="input"
            id="email"
            placeholder="toluola7@gmail.com"
          />
        </div>
        <div>
          <label for="password" class="label">Password</label><br />
          <input
            type="password"
            v-model="password"
            class="input"
            id="password"
            placeholder="Password"
          />
        </div>
        <button
          type="submit"
          class="signin-button"
        >Sign In</button>

        <div class="link-grey">
          <router-link to="/signup" class="link-grey">Sign up</router-link>
        </div>
      </form>
      </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Signin',
  data () {
    return {
      email: '',
      password: '',
      error: ''
    }
  },
  created () {
    this.checkSignedIn()
  },
  updated () {
    this.checkSignedIn()
  },
  methods: {
    signin () {
      this.$http.plain.post('/signin', { email: this.email, password: this.password })
        .then(response => this.signinSuccessful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccessful (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/records')
    },
    signinFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || ''
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkSignedIn () {
      if (localStorage.signedIn) {
        this.$router.replace('/records')
      }
    }
  }
}
</script>

<style>
  .signin-form-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .signin-form {
    border: #f2f2f2 solid 2px;
    border-radius: 1px;
    width: 300px;
    height: 350px;
    padding-left: 50px;
    box-shadow: 1px 1px 1px 1px #edf1f7;
  }

  .input {
    width: 250px;
    padding-left: 10px;
    margin-bottom: 7px;
    margin-top: 3px;
    height: 25px;
    border: 0px;
    border-radius: 3px;
    background: #edf1f7;
    font-size: 12px;
  }

  .signin-button {
    background: #57bd44;
    border: none;
    border-radius: 3px;
    width: 260px;
    height: 30px;
    font-size: 15px;
    color: white;
    margin-top: 10px;
  }

  .link-grey {
    margin-top: 8px;
    color: #a3a2a2;
  }

  .label {
    font-size: 13px;
  }
</style>
