<template>
  <div style="display: flex; justify-content: center">
    <div
      style="
        background-color: green;
        padding: 10px;
        border-radius: 5px;
        margin-top: 20px;
      "
    >
      <h4 style="color: white">
        Barang Sudah Terbayar
        <i class="bi bi-bag-check"></i>
      </h4>
    </div>
  </div>

  <router-link style="margin-left: 5px" :to="{ name: 'components.home' }"
    >Home</router-link
  >
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";
export default {
  setup() {
    //vue router
    const router = useRouter();
    function goHome() {
      router.push({
        name: "barang.sudahTerbayar",
      });
    }
    //return
    return {
      goHome,
    };
  },
};
</script>