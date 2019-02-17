<template>
  <div id="app">
    <div class="table-wrapper">
      <GTable
        :data="tableData"
        filtered
        paginated
      >
        <template
          slot="row-actions"
          slot-scope="props"
        >
          <button
            class="edit-btn"
            @click="onClickEditBtn(props.row)"
          >
            Edit
          </button>
        </template>
      </GTable>
    </div>
  </div>
</template>

<script>
import GTable from '@/components/GTable'
import tableData from '../data.json'

const EDIT_ALERT_TEXT = `Hi!
Check your brower console, there you can find full object of row(table item) which you want to edit.
So you can do everything in this scope(outside the table component) using slots`

export default {
  components: { GTable },
  data() {
    return {
      tableData,
    }
  },
  methods: {
    onClickEditBtn(item) {
      console.log(item)
      // eslint-disable-next-line no-alert
      alert(EDIT_ALERT_TEXT)
    },
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
</style>
