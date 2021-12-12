<template>
  <ul class="flex justify-center">
  <li v-for="(n,i) in Math.round(rate)" :key="i">
    <i class="fas fa-star fa-sm text-yellow-500 mr-1"></i>
  </li>
  <li  v-for="(n,i) in 5-Math.round(rate)" :key="i">
    <i class="far fa-star fa-sm text-yellow-500 mr-1"></i>
  </li>
</ul>
</template>

<script>
export default {
    props:["rate"]
}
</script>

<style>

</style>