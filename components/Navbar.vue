<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <b-navbar-brand href="/">Kulineran</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <nuxt-link to="/" class="nav-link">Home</nuxt-link>
            </li>
            <li class="nav-item">
              <nuxt-link to="/food" class="nav-link">Foods</nuxt-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <nuxt-link to="/keranjang" class="nav-link"
                >Keranjang
                <span class="badge badge-success ml-2">{{
                  jumlah_pemesanan.length
                }}</span>
              </nuxt-link>
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: 'NavbarComponent',
  data() {
    return {
      jumlah_pemesanan: [],
    }
  },
  mounted() {
    // Make a request using GET
    this.$axios
      .get('keranjangs')
      .then((response) => {
        // handle success
        console.log('Berhasil jumlah product di keranjang : ', response)
        this.setJumlah(response.data)
      })
      .catch((error) => {
        // handle error
        console.log('Gagal jumlah product di keranjang : ', error)
      })
  },
  methods: {
    setJumlah(data) {
      this.jumlah_pemesanan = data
    },
  },
}
</script>

<style>
</style>