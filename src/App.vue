<template>
  <div id="app">
    <m-table></m-table>
  </div>
</template>

<script>
import MTable from './components/mTable.vue';

export default {
  name: 'App',
  components: {
    MTable
  },
  data(){
    return {}
  },
  methods:{
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
