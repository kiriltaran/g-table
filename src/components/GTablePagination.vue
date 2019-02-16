<template>
  <div class="g-table-pagination">
    <ul class="g-table-pagination__list">
      <li
        v-for="(page, index) in pages"
        :key="index"
        :class="['list-item', { 'list-item--active': isActivePage(page) }]"
      >
        <span
          v-if="isDivider(page)"
          class="divider"
        >
          ...
        </span>
        <button
          v-else
          @click="changePage(page)"
        >
          {{ page }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
import { range } from 'ramda'

const MAX_VISIBLE_PAGES = 5
export default {
  name: 'GTablePagination',
  props: {
    value: {
      type: Number,
      required: true,
    },
    totalItems: {
      type: Number,
      required: true,
    },
    perPage: {
      type: Number,
      required: true,
    },
  },
  computed: {
    pages() {
      if (this.totalPages <= MAX_VISIBLE_PAGES + 1) {
        return range(1, this.totalPages + 1)
      }
      if (this.value < MAX_VISIBLE_PAGES) {
        return [...range(1, MAX_VISIBLE_PAGES + 1), 'divider', this.totalPages]
      }
      if (this.totalPages - MAX_VISIBLE_PAGES + 1 < this.value) {
        return [
          1,
          'divider',
          ...range(this.totalPages - MAX_VISIBLE_PAGES + 1, this.totalPages + 1),
        ]
      }
      return [1, 'divider', this.value - 1, this.value, this.value + 1, 'divider', this.totalPages]
    },
    totalPages() {
      return Math.ceil(this.totalItems / this.perPage)
    },
  },
  watch: {
    totalItems() {
      this.changePage(1)
    },
  },
  methods: {
    changePage(newPage) {
      this.$emit('input', newPage)
    },
    isActivePage(page) {
      return this.value === page
    },
    isDivider(page) {
      return page === 'divider'
    },
  },
}
</script>

<style lang="scss" scoped>
.g-table-pagination {
  &__list {
    display: flex;
    list-style-type: none;
  }
}

.list-item {
  &--active {
    transform: scale(1.2);
    background: none;
    transition: transform 300ms;
  }
}

.divider {
  margin: 0 5px;
}
</style>
