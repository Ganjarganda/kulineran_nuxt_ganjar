<template>
  <div class="keranjang">
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <router-link to="/pesanan-sukses">
            <span class="badge badge-success">Lihat checkout</span>
          </router-link>
          <div class="table-responseive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th scope="row">{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="keranjang.product_dipesan.gambar"
                      class="img-fluid shadow"
                      width="100"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.product_dipesan.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : '-' }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td align="right">Rp{{ keranjang.product_dipesan.harga }}</td>
                  <td align="right">
                    <strong
                      >Rp
                      {{
                        keranjang.jumlah_pemesanan *
                        keranjang.product_dipesan.harga
                      }}
                    </strong>
                  </td>
                  <td class="text-danger" align="center">
                    <button
                      type="text"
                      class="btn btn-danger float-right"
                      @click="hapusKeranjang(keranjang.id)"
                    >
                      Hapus
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right"><strong>Total :</strong></td>
                  <td align="right">
                    <strong>Rp{{ totalHarga }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <!-- Form Checkout -->
      <div v-show="show_form" class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" @submit.prevent="checkout">
            <div class="form-group">
              <label for="nama">Nama Pemesan</label>
              <input
                v-model="pesan.nama"
                type="text"
                class="form-control"
                placeholder="Contoh Ade"
              />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja</label>
              <input
                v-model="pesan.noMeja"
                type="text"
                class="form-control"
                placeholder="Contoh 20"
              />
            </div>
            <button type="submit" class="btn btn-success float-right">
              Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'KeranjangView',
  data() {
    return {
      keranjangs: [],
      informasi: '',
      pesan: {},
      show_form: false,
    }
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.product_dipesan.harga * data.jumlah_pemesanan
      }, 0)
    },
  },
  mounted() {
    this.viewDatakeranjang()
  },
  methods: {
    setKeranjang(data) {
      if (data.length === 0) {
        this.show_form = false
      } else {
        this.show_form = true
      }
      this.keranjangs = data
    },
    viewDatakeranjang() {
      // Make a request using GET
      this.$axios
        .get('keranjangs')
        .then((response) => {
          // handle success
          console.log('Berhasil data product di keranjang : ', response)
          this.setKeranjang(response.data)
        })
        .catch((error) => {
          // handle error
          console.log('Gagal data product di keranjang : ', error)
        })
    },
    hapusKeranjang(id) {
      // Make a request using DELETE
      this.$axios
        .delete('keranjangs/' + id)
        .then((response) => {
          // handle success
          console.log('Berhasil hapus data product di keranjang : ', response)
          this.informasi = 'Pesanan telah di hapus'
          this.$toast.success(this.informasi + ' (1)')
          // view ulang data keranjang
          this.viewDatakeranjang()
        })
        .catch((error) => {
          // handle error
          console.log('Gagal hapus data product di keranjang : ', error)
          this.informasi = 'Pesanan gagal di hapus'
          this.$toast.error(this.informasi + ' (2)')
        })
    },
    checkout() {
      console.log('Pesan :', this.pesan)
      if (this.pesan.nama && this.pesan.noMeja) {
        this.pesan.keranjangs = this.keranjangs
        // Make a request using POST
        this.$axios
          .post('pesanans', this.pesan)
          .then((response) => {
            // handle success
            console.log('Berhasil di pesan : ', response)

            this.informasi = 'Sukses dipesan'
            this.$toast.success(this.informasi + ' (3)')

            // hapus isi keranjang
            this.keranjangs.map(function (item) {
              // Make a request using DELETE
              return this.$axios
                .delete('keranjangs/' + item.id)
                .catch((error) =>
                  // handle error
                  console.log('Gagal hapus data product di keranjang : ', error)
                )
            })

            // pergi ke ui pesanan sukses
            this.$router.push({ path: '/pesanan-sukses' })
          })
          .catch((error) => {
            // handle error
            console.log('Gagal di pesan : ', error)
            this.$toast.error(this.informasi + ' (4)')
          })
      } else {
        this.informasi = 'Nama dan nomor meja mohon untuk diisi'
        this.$toast.error(this.informasi + ' (2)')
      }
    },
  },
}
</script>

<style>
</style>