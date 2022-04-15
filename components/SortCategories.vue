<template>
  <div
    class="d-flex flex-row justify-content-between align-items-center mt-3 mb-4"
  >
    <select
      v-model="selectedItem"
      class="form-select w-75"
      aria-label="Default select example"
      @change="selectCategory"
    >
      <option selected disabled value="">Choose category</option>
      <option v-for="category of categories" :key="category" :value="category">
        {{ category }}
      </option>
    </select>
    <button class="btn btn-primary" type="submit" @click="showAllProducts">
      Show all
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      categories: null,
      selectedItem: '',
    }
  },
  async fetch() {
    await this.getAllCategories()
  },
  methods: {
    async getAllCategories() {
      this.categories = await fetch(
        'https://dummyjson.com/products/categories'
      ).then((res) => res.json())
    },
    selectCategory(event) {
      this.$emit('selectedItem', event.target.value)
    },
    showAllProducts() {
      this.$emit('showAll')
    },
  },
}
</script>

<style></style>
