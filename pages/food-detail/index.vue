<template>
  <div class="food-detail">
    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <nuxt-link to="/" class="text-dark">Home</nuxt-link>
              </li>
              <li class="breadcrumb-item">
                <nuxt-link to="/food" class="text-dark">Foods</nuxt-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row mt-3">
        <div class="col-md-6">
          <img class="img-fluid shadow" :src="product.gambar" alt="" />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga: <strong>Rp{{ product.harga }}</strong>
          </h4>
          <form class="mt-4" @submit.prevent="pemesanan">
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesan</label>
              <input
                v-model="pesan.jumlah_pemesanan"
                type="number"
                class="form-control"
                placeholder="1"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                v-model="pesan.keterangan"
                class="form-control"
                placeholder="Keterangan contoh Pedas, Nasi setengah dll..."
              />
            </div>
            <button type="submit" class="btn btn-success">Pesan</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FoodDetailPage',
  data() {
    return {
      product: {},
      pesan: {},
      informasi: '',
    }
  },
  mounted() {
    // Make a request using GET
    this.$axios
      .get('products/' + this.$route.params.id)
      .then((response) => {
        // handle success
        console.log('Berhasil : ', response)
        this.setProduct(response.data)
      })
      .catch((error) => {
        // handle error
        console.log('Gagal : ', error)
      })
  },
  methods: {
    setProduct(data) {
      this.product = data
    },
    pemesanan() {
      if (this.pesan.jumlah_pemesanan == null) {
        this.informasi =
          'Ups, mohon masukkan jumlah pemesanan dengan benar dan terisi'
        console.log(this.informasi)
        this.$toast.error(this.informasi + ' (1)')
      } else {
        console.log('cek pemesanan: ', this.pesan)
        this.pesan.product_dipesan = this.product
        // kirim ke keranjang
        // Make a request using POST
        this.$axios
          .post('keranjangs', this.pesan)
          .then((response) => {
            // handle success
            console.log('Kirim ke keranjang: Berhasil : ', response)
            this.informasi = 'Pemesanan telah masuk ke keranjang'
            this.$toast.success(this.informasi + ' (2)')
            this.$router.push({ path: '/keranjang' })
          })
          .catch((error) => {
            // handle error
            console.log('Kirim ke keranjang: Gagal : ', error)
            this.$toast.error(error + ' (3)')
          })
      }
    },
  },
}
</script>

<style>
</style>