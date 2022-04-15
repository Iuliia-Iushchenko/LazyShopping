<template>
  <div class="d-flex flex-column justify-content-center">
    <Loading v-if="$fetchState.pending" />
    <div v-else class="card mb-3 mx-auto mt-5" style="max-width: 540px">
      <div class="row g-0">
        <div class="col-md-4">
          <img
            :src="`${product.images[0]}`"
            class="img-fluid rounded-start"
            alt="..."
          />
        </div>
        <div class="col-md-8">
          <div class="card-body">
            <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text">
              {{ product.description }}
            </p>
            <p class="card-text">
              <small class="text-muted">{{ product.price }} ₽</small>
            </p>
          </div>
        </div>
      </div>
    </div>
    <NuxtLink to="/" class="btn btn-outline-warning align-self-center mt-3"
      >Back to Home page</NuxtLink
    >
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
    }
  },
  async fetch() {
    await this.getSingleProduct()
  },
  head() {
    return {
      title: this.product.title,
    }
  },
  methods: {
    async getSingleProduct() {
      this.product = await fetch(
        'https://dummyjson.com/products/' + this.$route.params.id
      ).then((res) => res.json())
    },
  },
}
</script>

<style></style>
