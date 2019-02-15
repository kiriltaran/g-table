<template>
  <div id="app">
    <div class="table-wrapper">
      <GTable :data="tableData" />
    </div>
  </div>
</template>

<script>
import GTable from '@/components/GTable.vue'
import tableData from '../data.json'

export default {
  components: { GTable },
  data() {
    return {
      tableData,
    }
  },
}
</script>


<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

.table-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
