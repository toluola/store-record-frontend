<template>
  <div class="signin-form-container">
    <div class="signin-form">
      <h3 class="">Sign Up</h3>
      <form @submit.prevent="signup">
        <div class="" v-if="error">{{ error }}</div>

        <div class="">
          <label for="email" class="label">E-mail Address</label>
          <input type="email" v-model="email" class="input" id="email" placeholder="toluola7@gmail.com">
        </div>

        <div class="">
          <label for="password" class="label">Password</label><br />
          <input type="password" v-model="password" class="input" id="password" placeholder="Password">
        </div>

        <div class="">
          <label for="password_confirmation" class="label">Password Confirmation</label>
          <input type="password" v-model="password_confirmation" class="input" id="password_confirmation" placeholder="Password Confirmation">
        </div>
        <button type="submit" class="signin-button">Sign Up</button>

        <div class="link-grey"><router-link to="/" class="link-grey">Sign In</router-link></div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Signup',
  data () {
    return {
      email: '',
      password: '',
      password_confirmation: '',
      error: ''
    }
  },
  created () {
    this.checkedSignedIn()
  },
  updated () {
    this.checkedSignedIn()
  },
  methods: {
    signup () {
      this.$http.plain.post('/signup', { email: this.email, password: this.password, password_confirmation: this.password_confirmation })
        .then(response => this.signupSuccessful(response))
        .catch(error => this.signupFailed(error))
    },
    signupSuccessful (response) {
      if (!response.data.csrf) {
        this.signupFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/records')
    },
    signupFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || 'Something went wrong'
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkedSignedIn () {
      if (localStorage.signedIn) {
        this.$router.replace('/records')
      }
    }
  }
}
</script>
