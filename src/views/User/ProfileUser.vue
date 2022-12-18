<template>
  <div class="card h-50 m-4 card text-dark bg-light mb-3">
    <div
      style="
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin: 10px;
      "
    >
      <h2>Profil User</h2>
      <br />
      <h5>{{ user.namaUser }}</h5>
      <p>{{ user.emailUser }}</p>
      <router-link :to="{ name: 'user.editUser' }" class="btn btn-primary"
        >Edit</router-link
      >
    </div>
  </div>
</template>

<script>
import { reactive, onMounted } from "vue";
export default {
  setup() {
    const user = reactive({
      namaUser: "",
      emailUser: "",
    });
    //mounted
    onMounted(() => {
      user.namaUser = localStorage.getItem("name");
      user.emailUser = localStorage.getItem("email");
    });
    //return
    return {
      user,
    };
  },
};
</script>