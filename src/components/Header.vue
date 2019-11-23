<template>
  <div class="header-container">
    <div class="header-subcontainer">
      <div class="header-text">
        <a href="/" class="header-logo">Record-Store</a>
      </div>
      <div class="header-link header-text">
        <router-link to="/" class="signin-link" v-if="!signedIn()">Sign In</router-link>
        <router-link to="/signup" class="signup-link" v-if="!signedIn()">| Sign Up</router-link>
        <router-link to="/records" class="record-link" v-if="signedIn()">Records</router-link>
        <router-link to="/artists" class="artist-link" v-if="signedIn()">| Artists</router-link>
        <a href="#" @click.prevent="signOut" class="signout-link" v-if="signedIn()">| Sign Out</a>
      </div>
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
    height: 60px;
    padding-left: 250px;
    margin-top: -10px;
    padding-top: 20px;
    margin-left: -10px;
    margin-right: -10px;
    margin-bottom: 20px;
  }

  .header-text {
    font-size: 20px;
    display: inline;
    text-decoration: none;
  }

  .header-logo {
    text-decoration: none;
  }

  .header-link {
    margin-left: 550px;
  }

  .signup-link, .signin-link, .artist-link, .signout-link, .record-link {
    text-decoration: none;
    color: #551A8B;
  }

  @media (max-width: 600px) {
    .header-container {
      padding-left: 15px;
      height: 35px;
      padding-top: 5px;
    }

    .header-link {
      margin-left: 20px;
    }

    .header-text {
      font-size: 10px;
    }
  }

  @media (max-width: 800px) {
    .header-container {
      padding-left: 45px;
      padding-top: 12px;
      height: 40px;
    }

    .header-link {
      margin-left: 180px;
    }

    .header-text {
      font-size: 20px;
    }
  }

</style>
