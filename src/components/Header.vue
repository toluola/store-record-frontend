<template>
  <div class="header-container">
    <div class="header-subcontainer">
      <div></div>
      <div class="header-text">
        <a href="/" class="header-logo">Record Store</a>
      </div>
      <div></div>
      <div class="header-link header-text">
        <router-link to="/" class="signin-link" v-if="!signedIn()">Sign In</router-link>
        <router-link to="/signup" class="signup-link" v-if="!signedIn()">Sign Up</router-link>
        <router-link to="/records" class="record-link" v-if="signedIn()">Records</router-link>
        <router-link to="/artists" class="artist-link" v-if="signedIn()">Artists</router-link>
        <a href="#" @click.prevent="signOut" class="signout-link" v-if="signedIn()">Sign Out</a>
      </div>
      <div></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',
  created () {
    this.signedIn()
  },
  methods: {
    setError (error, text) {
      this.error = (error.response && error.response.data && error.response.data.error) || text
    },
    signedIn () {
      return localStorage.signedIn
    },
    signOut () {
      this.$http.secured.delete('/signin')
        .then(response => {
          delete localStorage.csrf
          delete localStorage.signedIn
          this.$router.replace('/')
        })
        .catch(error => this.setError(error, 'Cannot sign out'))
    }
  }
}
</script>

<style scoped>
  .header-container {
    background: #edf1f7;
    margin-top: -10px;
    height: 50px;
    padding-top: 18px;
    margin-bottom: 20px;
    margin-left: -25px;
    margin-right: -25px;
  }

  .header-subcontainer {
    display: grid;
    background: #edf1f7;
    grid-template-columns: 10% 15% 50% 15% 10%;
  }

  .header-text {
    font-size: 14px;
    text-decoration: none;
  }

  .header-logo {
    text-decoration: none;
  }

  .header-link {
    display: grid;
    grid-template-columns: 50% 50%;
    margin-right: -20px;
  }

  .signup-link, .signin-link, .artist-link , .signout-link {
    text-decoration: none;
  }

</style>
