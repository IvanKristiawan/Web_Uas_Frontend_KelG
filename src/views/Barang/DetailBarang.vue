<template>
  <div width="100">
    <img
      v-bind:src="barang.gambarBarang"
      class="card-img-top"
      alt="asd"
      width="50"
    />
  </div>
  <h2>{{ barang.namaBarang }}</h2>
  <p>{{ barang.hargaBarang }}</p>
  <p>{{ barang.deskripsiBarang }}</p>

  <div v-for="(review, id) in reviews" :key="id">
    <div class="col">
      <p class="card-title">{{ review.namaUser }}</p>
      <p class="card-title">Rating: {{ review.rating }}</p>
    </div>
  </div>

  <form @submit.prevent="store">
    <div class="form-group mb-3">
      <label for="content" class="form-label">Masukkan Rating</label>
      <input
        class="form-control"
        type="number"
        v-model="tempRating.rating"
        placeholder="Masukkan rating"
      />
    </div>
    <button type="submit" class="btn btn-primary">SIMPAN REVIEW</button>
  </form>

  <form @submit.prevent="masukKeranjang">
    <button type="submit" class="btn btn-primary">Masukkan Ke Keranjang</button>
  </form>
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";
export default {
  setup() {
    const tempRating = reactive({
      rating: "",
    });
    const route = useRoute();
    const id = route.params.id;
    //state barang
    const barang = reactive({
      namaBarang: "",
      hargaBarang: "",
      deskripsiBarang: "",
      gambarBarang: "",
    });
    let reviews = ref([]);
    //state validation
    const validation = ref([]);
    //vue router
    const router = useRouter();

    onMounted(() => {
      //get API from Laravel Backend
      axios
        .get(`https://tubesuaswebbackend.itkitapro.com/api/barang/${id}`)
        .then((response) => {
          //assign state posts with response data
          barang.namaBarang = response.data.data.namaBarang;
          barang.hargaBarang = response.data.data.hargaBarang;
          barang.deskripsiBarang = response.data.data.deskripsiBarang;
          barang.gambarBarang = response.data.data.gambarBarang;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
      axios
        .get(
          `https://tubesuaswebbackend.itkitapro.com/api/reviewByIdBarang/${id}`
        )
        .then((response) => {
          //assign state posts with response data
          reviews.value = response.data.data;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });
    function store() {
      let rating = tempRating.rating;
      axios
        .post("https://tubesuaswebbackend.itkitapro.com/api/review", {
          rating: rating,
          idUser: localStorage.getItem("id"),
          namaUser: localStorage.getItem("name"),
          gambarBarang: barang.gambarBarang,
          kuantitas: 1,
          idBarang: id,
        })
        .then((response) => {
          //redirect ke post index
          router.push({
            name: "components.home",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.data;
        });
    }
    function masukKeranjang() {
      axios
        .post("https://tubesuaswebbackend.itkitapro.com/api/keranjang", {
          idBarang: id,
          idUser: localStorage.getItem("id"),
          gambarBarang: barang.gambarBarang,
          kuantitas: 1,
          namaBarang: barang.namaBarang,
        })
        .then((response) => {
          //redirect ke post index
          router.push({
            name: "barang.keranjang",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.data;
        });
    }
    //return
    return {
      reviews,
      barang,
      validation,
      router,
      store,
      tempRating,
      masukKeranjang,
    };
  },
};
</script>