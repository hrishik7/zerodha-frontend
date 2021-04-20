<template>
  <div id="app">
    <HelloWorld msg="Hello"/>
    <form @submit.prevent="fetchData">
      <input v-model="search_key" placeholder="Search name here">
      <button>Search</button>
    </form>
    <download-csv
      :data="equity_data">
      <button>Download Data</button>
    </download-csv>    
    <b-table hover :items="equity_data"></b-table>
    <button @click="decrementPage">Prev</button>
    <button @click="incrementPage">Next</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      equity_data : [],
      search_key : "",
      page: 1,
    };
  },
  methods: {
    fetchData: function(){
      axios.get("http://localhost:8000/new/?s=" + this.search_key + "&page=" + this.page)
      .then(res => {
        console.log(res)
        this.equity_data = res.data
      })
      .catch(err => {
        console.error(err)
      })
    },
    decrementPage: function(){
      if (this.page > 1){
        this.page -= 1;
        this.fetchData()
      }
    },
    incrementPage: function(){
      this.page += 1;
      this.fetchData()
    }
  },
  mounted() {
    this.fetchData()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
