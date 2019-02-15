<template>
  <div class="g-table">
    <input
      v-model="filterText"
      type="text"
      class="g-table__input"
    >
    <table class="g-table__table">
      <thead>
        <tr>
          <th
            v-for="(title, key) in data.titles"
            :key="key"
            class="table-head"
            :class="key === sortKey ? sortClass : null"
            @click="onClickTitle(key)"
          >
            {{ title }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in dataItems"
          :key="item.name"
          class="table-row"
        >
          <td
            v-for="(value, key) in item"
            :key="key"
            class="table-cell"
          >
            {{ value }}
          </td>
          <td>
            <button
              class="edit-btn"
              @click="onClickEditBtn(item)"
            >
              Edit
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { descend, ascend, sort, prop } from 'ramda'

export default {
  name: 'GTable',
  props: {
    data: {
      type: Object,
      required: true,
      validator(value) {
        return value && value.titles && value.items
      },
    },
  },
  data() {
    return {
      filterText: '',
      dataItems: [...this.data.items],
      descendSort: true,
      sortKey: null,
    }
  },
  computed: {
    sortClass() {
      return this.descendSort ? 'table-head--sort-desc' : 'table-head--sort-asc'
    },
  },
  watch: {
    filterText() {
      this.filterDataItems()
    },
  },
  methods: {
    filterDataItems() {
      const filteredDataItems = this.dataItems.filter(item =>
        Object.values(item).some(value => value.includes(this.filterText)),
      )

      this.dataItems = this.filterText === '' ? [...this.data.items] : [...filteredDataItems]
    },
    sortDataItems(key) {
      const orderSortFn = this.descendSort ? descend : ascend
      this.dataItems = [...sort(orderSortFn(prop(key)), this.dataItems)]
    },
    onClickTitle(key) {
      if (this.sortKey === key) {
        this.descendSort = !this.descendSort
      } else {
        this.sortKey = key
        this.descendSort = true
      }

      this.sortDataItems(key)
    },
    onClickEditBtn(item) {
      // eslint-disable-next-line no-alert
      alert('TODO: item editing')
      console.log(item)
    },
  },
}
</script>

<style lang="scss" scoped>
*,
*:after,
*:before {
  position: relative;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.g-table {
  &__input {
    line-height: 22px;
    font-size: 22px;
    border: 1px solid #333;
    margin-bottom: 20px;
  }

  &__table {
    border-collapse: collapse;
  }
}

.table-head {
  min-width: 150px;
  background-color: #e0e0e0;
  color: #333;
  text-transform: uppercase;
  white-space: nowrap;
  font-size: 0.85em;
  padding: 2px;

  &:hover {
    cursor: pointer;
  }

  &--sort-asc:after,
  &--sort-desc:after {
    position: absolute;
    right: 5px;
    display: inline-block;
    color: #333;
  }

  &--sort-asc:after {
    content: '↑';
  }

  &--sort-desc:after {
    content: '↓';
  }
}

.table-row {
  line-height: 25px;
}

.table-cell {
  border-top: 1px solid #eee;
  border-bottom: 1px solid #eee;
}

.edit-btn {
  min-width: 60px;
}
</style>
