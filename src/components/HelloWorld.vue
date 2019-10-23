<template>
  <div class="hello">
    <h1>{{ fruitsArray }}</h1>
  </div>  
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data() {
    return {
      fruitsArray: []
    }
  },
  methods: {
    getData: function() {
      var vm = this
      axios.get("http://localhost:3000/fruit", {
        headers: {
          Authorization: 'Bearer SOME_TOKEN' 
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
