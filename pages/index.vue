<template>
  <div class="mb-5">
    <!-- Search -->
    <nav class="navbar navbar-light bg-light mt-4">
      <div class="container-fluid">
        <div class="d-flex">
          <input
            v-model.lazy="searchInput"
            class="form-control me-2"
            type="search"
            placeholder="Search"
            aria-label="Search"
            @keyup.enter="$fetch"
          />
          <button
            v-show="searchInput !== ''"
            class="btn btn-outline-primary text-nowrap"
            @click="clearSearch"
          >
            Clear Search
          </button>
        </div>
      </div>
    </nav>

    <SortCategories
      @selectedItem="selectedCategory"
      @showAll="showAllProducts"
    />

    <Loading v-if="isLoading" />

    <div v-else>
      <nav
        v-show="isAllproducts"
        aria-label="Page navigation example"
        class="mt-3"
      >
        <ul class="pagination justify-content-end">
          <li
            v-for="pageNumber in totalPages"
            :key="pageNumber"
            class="page-item"
            :class="{ active: page === pageNumber }"
          >
            <a class="page-link" href="#" @click="changePage(pageNumber)">{{
              pageNumber
            }}</a>
          </li>
        </ul>
      </nav>

      <div
        v-if="selectedProducts.length !== 0"
        class="row row-cols-1 row-cols-md-5 g-4 mt-4"
      >
        <ProductItem
          v-for="product of selectedProducts"
          v-show="!isAllproducts"
          :key="product.id"
          :product="product"
        />
      </div>

      <!-- Searched Products -->
      <div
        v-if="searchInput !== ''"
        class="row row-cols-1 row-cols-md-5 g-4 mt-4"
      >
        <ProductItem
          v-for="product of searchedProducts"
          :key="product.id"
          :product="product"
        />
      </div>

      <!-- All products -->
      <div v-show="isAllproducts" class="row row-cols-1 row-cols-md-5 g-4 mt-4">
        <ProductItem
          v-for="product of products"
          :key="product.id"
          :product="product"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      searchedProducts: [],
      selectedProducts: [],
      searchInput: '',
      isAllproducts: true,
      isLoading: false,

      page: 1,
      skip: 0,
      limit: 10,
      totalPages: 0,
    }
  },
  async fetch() {
    if (this.searchInput === '') {
      await this.getProducts()
      return
    }
    await this.searchProducts()
  },
  head() {
    return {
      title: 'LazyShoping App',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Show products from Dummyjson API',
        },
        {
          hid: 'keywords',
          name: 'keywords',
          content: 'online shop, shopping, marketplace',
        },
      ],
    }
  },
  methods: {
    async getProducts() {
      try {
        this.isLoading = true
        this.products = await fetch(
          `https://dummyjson.com/products?limit=${this.limit}&skip=${this.skip}`
        )
          .then((res) => res.json())
          .then((res) => {
            this.totalPages = Math.ceil(res.total / this.limit)
            this.isAllproducts = true
            return res.products
          })
      } catch (e) {
        // eslint-disable-next-line no-console
        console.log(e)
      } finally {
        this.isLoading = false
      }
    },
    async searchProducts() {
      try {
        this.isLoading = true
        this.searchedProducts = await fetch(
          `https://dummyjson.com/products/search?q=${this.searchInput}`
        )
          .then((res) => res.json())
          .then((res) => {
            this.selectedProducts = []
            this.isAllproducts = false
            return res.products
          })
      } catch (e) {
        // eslint-disable-next-line no-console
        console.log(e)
      } finally {
        this.isLoading = false
      }
    },
    async selectedCategory(selectedItem) {
      try {
        this.isLoading = true
        this.selectedProducts = await fetch(
          `https://dummyjson.com/products/category/${selectedItem}`
        )
          .then((res) => res.json())
          .then((res) => {
            this.isAllproducts = false
            this.searchedProducts = []
            this.searchInput = ''
            return res.products
          })
      } catch (e) {
        // eslint-disable-next-line no-console
        console.log(e)
      } finally {
        this.isLoading = false
      }
    },
    clearSearch() {
      this.searchInput = ''
      this.searchedProducts = []
      this.page = 1
      this.skip = 0
      this.isAllproducts = true
    },
    changePage(currentPage) {
      this.page = currentPage
      this.skip = (currentPage - 1) * this.limit
      this.getProducts()
    },
    showAllProducts() {
      this.searchInput = ''
      this.changePage(1)
    },
  },
}
</script>
