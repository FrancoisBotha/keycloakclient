<template>
  <div class="hello">
    <h1>OpenID Connect Client</h1>
    <button v-on:click="authenticate">Authenticate</button>
    <h3> Access Token: {{ AccessToken }}</h3>
    <h4>{{ fruitsArray }}</h4>
  </div>  
</template>

<script>
import axios from 'axios'
import * as Keycloak from 'keycloak-js'

export default {
  name: 'HelloWorld',
  data() {
    return {
      fruitsArray: [],
      AccessToken: ""
    }
  },
  methods: {
    authenticate: function() {

      //keycloak init options
      let kcOptions = {
        url: 'http://localhost:8085/auth', 
        realm: 'AuthSrvTest', 
        clientId: 'vueclient'
      }

      let keycloak = Keycloak(kcOptions)

      this.AccessToken = "Authenticating...."

      keycloak.init({ onLoad: 'login-required' }).success((auth) =>{
          
          if(!auth) {
            console.log("NOT Authenticated")
            this.AccessToken = ""
          } else {
            console.log("Authenticated")
            this.AccessToken = keycloak.token
          }
      
          localStorage.setItem("vue-token", keycloak.token)
          localStorage.setItem("vue-refresh-token", keycloak.refreshToken)

          this.getData()

      }).error(() =>{
        console.log("Authenticated Failed")
      });
    },    
    getData: function() {
      var vm = this
      axios.get("http://localhost:3000/fruit", {
        headers: {
          Authorization: 'Bearer ' + localStorage.getItem('vue-token')
      }}).then(function(res) {
              vm.fruitsArray = res.data
            })
            .catch(function(err) {
              alert(err)
            })
    }
  },
  created() {
    this.getData()
  }
}
</script>
