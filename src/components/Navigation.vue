<template>
  <head>
    <!-- Bootstrap -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT"
      crossorigin="anonymous"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-alpha1/dist/js/bootstrap.bundle.min.js"
    />
    <!-- Bootstrap Icon -->
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.9.1/font/bootstrap-icons.css"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.5.0/font/bootstrap-icons.css"
    />
    <!-- CSS only -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"
      crossorigin="anonymous"
    />
  </head>
  <nav class="navbar navbar-light bg-light">
    <div class="container-fluid">
      <router-link
        :to="{ name: 'components.home' }"
        class="btn btn-outline-primary bi-house-fill"
        >Home</router-link
      >
      <div class="d-flex my-2 my-lg-0">
        <router-link
          v-if="user.namaUser"
          :to="{
            name: 'barang.keranjang',
            params: { id: user.idUser },
          }"
          class="btn btn-outline-primary bi-basket-fill me-2"
        >
        </router-link>
        <router-link
          v-if="!user.namaUser"
          :to="{ name: 'user.login' }"
          class="btn btn-warning me-2"
          >Login</router-link
        >
        <router-link
          v-if="!user.namaUser"
          :to="{ name: 'user.register' }"
          class="btn btn-info me-2"
          >Register</router-link
        >
        <!-- <p>{{ user.namaUser }}</p> -->
        <form @submit.prevent="logout" v-if="user.namaUser">
          <button type="submit" class="btn btn-outline-primary me-2">
            Logout
          </button>
        </form>
        <router-link
          v-if="user.namaUser"
          :to="{ name: 'user.profilUser' }"
          class="btn btn-outline-primary bi-person-badge-fill me-2"
          >{{ user.namaUser }}</router-link
        >
      </div>
    </div>
  </nav>
  <main style="min-height: 100vh; border: 0.25px solid black">
    <router-view></router-view>
  </main>

  <footer class="absolute-bottom" style="background-color: aquamarine">
    <!-- Copyright -->
    <div class="text-center p-3">
      <a
        class="bi bi-instagram"
        href="#!"
        role="button"
        style="font-size: 40px; color: #dd4b39; padding-right: 3cm"
      >
      </a>

      <a
        class="bi bi-facebook"
        href="#!"
        role="button"
        style="font-size: 40px; color: lightskyblue; padding-right: 3cm"
      >
      </a>

      <a
        class="bi bi-twitter"
        href="#!"
        role="button"
        style="font-size: 40px; color: lightskyblue; padding-right: 3cm"
      >
      </a>
      <a style="padding-right: 3cm"> 035-336412 </a>
      <a style="padding-right: 3cm">About Us</a>
    </div>
    <div class="text-center p-3" style="background-color: rgba(0, 0, 0, 0.3)">
      E-Commerce Web by Kelompok G
    </div>
    <!-- Copyright -->
  </footer>
</template>
    
    <script>
import { reactive, onMounted } from "vue";
import { useRouter } from "vue-router";
export default {
  setup() {
    const user = reactive({
      idUser: "",
      namaUser: "",
    });
    const router = useRouter();
    //mounted
    onMounted(() => {
      user.idUser = localStorage.getItem("id");
      user.namaUser = localStorage.getItem("name");
    });
    function logout() {
      user.namaUser = null;
      router.push({
        name: "barang.login",
      });
    }
    //return
    return {
      user,
      logout,
    };
  },
};
</script>