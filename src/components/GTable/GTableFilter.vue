<template>
  <div class="g-table-filter">
    Search:
    <input
      v-model="filterText"
      class="g-table-filter__input"
    >
  </div>
</template>

<script>
export default {
  name: 'GTableFilter',
  props: {
    value: {
      type: String,
      default: '',
    },
  },
  computed: {
    filterText: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      },
    },
  },
}
</script>


<style lang="scss">
.g-table-filter {
  &__input {
    padding: 5px;
    outline: none;
    border: 1px solid #ddd;
  }
}
</style>
