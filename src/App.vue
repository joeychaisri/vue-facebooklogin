<template>
  <div id="app">
    <img src="./assets/logo.png"> <br>
    <button @click="sign()"> signin </button>
    <button @click="signOut()"> signout </button>
    <br>
    <img :src="photoURL"> <br>
    {{ displayName }}
    <router-view></router-view>
  </div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: "AIzaSyATyqGFn2D0jT4AF5uTVWtsH3mwAqxjTQ8",
    authDomain: "grouppe-4d101.firebaseapp.com",
    databaseURL: "https://grouppe-4d101.firebaseio.com",
    projectId: "grouppe-4d101",
    storageBucket: "grouppe-4d101.appspot.com",
    messagingSenderId: "514339265044"
}

firebase.initializeApp(config)
var provider = new firebase.auth.FacebookAuthProvider()
provider.addScope('public_profile')
provider.setCustomParameters({
  'display': 'popup'
})


export default {
  name: 'app',
  data () {
    return {
      displayName: '',
      photoURL: ''
    }
  },
  methods: {
    sign () {
      console.log('yes')
      var vm = this
      firebase.auth().signInWithPopup(provider).then(function (result) {
        var token = result.credential.accessToken
        var user = result.user
        console.log(user)
        // console.log('displayName :: ', user.displayName)
        // console.log('photoURL ::', user.photoURL)
        vm.displayName = user.displayName
        vm.photoURL = user.photoURL
      }).catch(function (error) {
        console.log(error)
      })
    },
    signOut () {
      var vm = this
      firebase.auth().signOut().then(function () {
        console.log('logOut')
        vm.displayName = ''
        vm.photoURL = ''
      }, function (error) {
        console.log(error)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>