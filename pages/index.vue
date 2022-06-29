<template>
  <div class="container">
    <Hero />
    <div class="row mt-4">
      <div class="col">
        <h2>Best <strong>Foods</strong></h2>
      </div>
      <div class="col">
         <nuxt-link to="/food" class="btn btn-success float-right">Lihat Semua</nuxt-link>
      </div>
    </div>
    <div class="row mb-4">
      <div
        v-for="item_product in products"
        :key="item_product.id"
        class="col-md-4 mt-4"
      >
        <CardProduct :item="item_product" />

      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      products: [],
    }
  },
  mounted() {
    // Make a request using GET
    this.$axios
      .get('best-products')
      .then((response) => {
        // handle success
        console.log('Berhasil : ', response)
        this.setProducts(response.data)
      })
      .catch((error) => {
        // handle error
        console.log('Gagal : ', error)
      })
  },
  methods: {
    setProducts(data) {
      this.products = data
    },
  },
}
</script>
