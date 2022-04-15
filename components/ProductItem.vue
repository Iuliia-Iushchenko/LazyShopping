<template>
  <div class="col">
    <div class="card h-100">
      <img
        :src="`${product.images[0]}`"
        style="height: 200px; object-fit: contain"
        :alt="`${product.title}`"
      />
      <div class="card-body">
        <h5 class="card-title">{{ product.title }}</h5>
      </div>
      <div class="card-footer">
        <p class="card-text">{{ product.price }} ₽</p>
        <nuxt-link :to="`/products/${product.id}`" class="btn btn-primary"
          >See more</nuxt-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: { product: Object },
}
</script>

<style></style>
