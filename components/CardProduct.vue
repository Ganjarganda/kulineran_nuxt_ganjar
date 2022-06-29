<template>
  <div class="card shadow card-product">
    <img :src="item.gambar" class="card-img-top" alt="..." />
    <div class="card-body">
      <h5 class="card-title">{{ item.nama }}</h5>
      <p class="card-text">Harga : Rp{{ item.harga }}</p>
      <!-- <a :href="'/food-detail/' + item.id" class="btn btn-success">Pesan</a> -->
      <nuxt-link :to="{name: 'food-detail', params: { id:item.id } }" class="btn btn-success">Pesan</nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardProductComponent',
  props: ['item'],
}
</script>

<style>
</style>