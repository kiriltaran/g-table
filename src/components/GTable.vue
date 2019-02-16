<template>
  <div class="g-table">
    <div class="g-table__top-row">
      <GTableFilter
        v-if="filtered"
        v-model="filterText"
        class="table-filter"
      />
    </div>
    <table class="g-table__table">
      <thead>
        <tr>
          <th
            v-for="(title, key) in data.titles"
            :key="key"
            :class="['table-head',{[sortClass]: key === sortKey}]"
            @click="onClickTitle(key)"
          >
            {{ title }}
          </th>
          <th class="table-head" />
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in localDataItems"
          :key="item.name"
          class="table-row"
        >
          <td
            v-for="(value, key) in item"
            :key="key"
            class="table-cell"
            v-html="getCellContent(value)"
          />
          <td class="table-cell table-cell--actions">
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
    <div class="g-table__top-row">
      <GTablePagination
        v-if="paginated"
        v-model="activePage"
        :total-items="localDataItems.length"
        class="table-pagination"
      />
    </div>
  </div>
</template>

<script>
import GTableFilter from '@/components/GTableFilter.vue'
import GTablePagination from '@/components/GTablePagination.vue'
import { descend, ascend, sort, prop, compose } from 'ramda'

export default {
  name: 'GTable',
  components: {
    GTableFilter,
    GTablePagination,
  },
  props: {
    data: {
      type: Object,
      required: true,
      validator(value) {
        return value && value.titles && value.items
      },
    },
    filtered: {
      type: Boolean,
      default: false,
    },
    paginated: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      localDataItems: [...this.data.items],
      filterText: '',
      descendSort: true,
      sortKey: null,
      activePage: 1,
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
      this.sortDataItems()
    },
  },
  methods: {
    filterDataItems() {
      const filteredDataItems = this.data.items.filter(item =>
        Object.values(item).some(value => value.includes(this.filterText)),
      )

      this.localDataItems = [...filteredDataItems]
    },
    sortDataItems() {
      const orderSortFn = this.descendSort ? descend : ascend
      const parsePropFn = this.getParsePropFn(this.sortKey)
      this.localDataItems = [...sort(orderSortFn(parsePropFn), this.localDataItems)]
    },
    onClickTitle(key) {
      if (this.sortKey === key) {
        this.descendSort = !this.descendSort
      } else {
        this.sortKey = key
        this.descendSort = true
      }

      this.sortDataItems()
    },
    onClickEditBtn(item) {
      // eslint-disable-next-line no-alert
      alert('edit', item)
    },
    getCellContent(data) {
      if (!this.filterText) {
        return data
      }

      const regex = new RegExp(this.filterText, 'g')
      const matched = data.match(regex) ? data.match(regex)[0] : null
      const replaced = data.replace(regex, `<span class="highlight">${matched}</span>`)
      return replaced
    },
    getParsePropFn(key) {
      // TODO: implement passing parse function for each table column by props
      switch (key) {
        case 'height':
          return compose(
            x => x || -1,
            parseFloat,
            prop(key),
          )

        case 'mass':
          return compose(
            x => x || -1,
            parseFloat,
            prop(key),
          )

        case 'birth_year':
          return compose(
            x => x || -1,
            parseFloat,
            prop(key),
          )

        default:
          return prop(key)
      }
    },
  },
}
</script>

<style lang="scss">
.g-table {
  &__top-row {
    display: flex;
    justify-content: flex-end;
    margin: 10px 0;
    padding: 0 5px;
  }

  &__table {
    width: 100%;
    border-collapse: collapse;
  }
}

.table-head {
  position: relative;
  min-width: 150px;
  background-color: #e0e0e0;
  color: #333;
  text-transform: uppercase;
  white-space: nowrap;
  font-size: 0.85em;
  line-height: 2em;

  &:not(:last-child) {
    border-right: 1px solid #333;
  }

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
  line-height: 1.6em;

  &:hover {
    background-color: #eee;
    .table-cell--actions {
      opacity: 1;
    }
  }
}

.table-cell {
  border-top: 1px solid #eee;
  border-bottom: 1px solid #eee;

  &--actions {
    opacity: 0;
  }
}

.edit-btn {
  min-width: 60px;
}

.highlight {
  background-color: aquamarine;
}
</style>
