<template>
  <div class="g-table-pagination">
    <ul class="g-table-pagination__list">
      <li
        v-for="(page, index) in pages"
        :key="index"
        :class="['list-item', { 'highlight': isCurrentPage(page) }]"
      >
        <button
          :disabled="isDotsButton(page)"
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
const MAX_VISIBLE_ITEMS = 10
const DIVIDERS_CONTENT = '..'
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
  },
  computed: {
    pages() {
      if (this.totalPages <= MAX_VISIBLE_PAGES + 1) {
        return range(1, this.totalPages + 1)
      }
      if (this.value < MAX_VISIBLE_PAGES) {
        return [...range(1, MAX_VISIBLE_PAGES + 1), DIVIDERS_CONTENT, this.totalPages]
      }
      if (this.totalPages - MAX_VISIBLE_PAGES + 1 < this.value) {
        return [
          1,
          DIVIDERS_CONTENT,
          ...range(this.totalPages - MAX_VISIBLE_PAGES + 1, this.totalPages + 1),
        ]
      }
      return [
        1,
        DIVIDERS_CONTENT,
        this.value - 1,
        this.value,
        this.value + 1,
        DIVIDERS_CONTENT,
        this.totalPages,
      ]
    },
    totalPages() {
      return Math.ceil(this.totalItems / MAX_VISIBLE_ITEMS)
    },
  },
  methods: {
    changePage(newPage) {
      this.$emit('input', newPage)
    },
    isCurrentPage(page) {
      return this.value === page
    },
    isDotsButton(page) {
      return page === DIVIDERS_CONTENT
    },
  },
}
</script>

<style lang="scss" scoped>
.highlight {
  transform: scale(1.3);
  background: none;
  transition: all 300ms;
}
.g-table-pagination {
  &__list {
    display: flex;
    list-style-type: none;
  }
}
</style>
