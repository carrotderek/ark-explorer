<template>
  <div class="mx-5 sm:mx-10 mt-5 md:mt-10 flex flex-wrap">
    <button @click="prev" :class="{ 'hidden' : page <= 1 }" class="mr-auto bg-theme-button hover:bg-blue text-theme-button-text semibold hover:text-white py-5 px-8 border-transparent rounded hover-button-shadow">
      Previous
    </button>
    <button @click="next" class="ml-auto bg-theme-button hover:bg-blue text-theme-button-text semibold hover:text-white py-5 px-8 border-transparent rounded hover-button-shadow float-right">
      Next
    </button>
  </div>
</template>

<script type="text/ecmascript-6">
export default {
  props: {
    start: {
      type: Number,
      require: true,
    },
  },

  data: () => ({
    page: 1,
  }),

  mounted() {
    this.page = this.start
  },

  methods: {
    prev() {
      if (this.page > 1) this.page--

      this.$parent.$emit('paginatorChanged', this.page)
    },

    next() {
      this.page++

      this.$parent.$emit('paginatorChanged', this.page)
    },
  },
}
</script>
