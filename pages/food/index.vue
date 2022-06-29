<template>
  <div class="foods">
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h3>Daftar <strong>Makanan</strong></h3>
        </div>
      </div>
      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              type="text"
              class="form-control"
              placeholder="Cari Makanan Kesukaan Anda..."
              aria-label="Cari"
              aria-describedby="basic-addon1"
              @keyup="searchFood"
            />
            <span id="basic-addon1" class="input-group-text">Cari</span>
          </div>
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
  </div>
</template>

<script>
export default {
  name: 'FoodPage',
  data() {
    return {
      products: [],
      search: '',
    }
  },
  mounted() {
    // Make a request using GET
    this.$axios
      .get('products')
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
    searchFood() {
      // Make a request using GET
      this.$axios
        .get('products?q=' + this.search)
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
  },
}
</script>

<style>
</style>