<template>
  <v-pagination v-model="currentPage" :length="totalPages" @input="changePage" />
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true
    },
    totalPages: {
      type: Number,
      required: true
    }
  },
  methods: {
    changePage(page) {
      this.$emit('page-changed', page)
    }
  }
}
</script>
