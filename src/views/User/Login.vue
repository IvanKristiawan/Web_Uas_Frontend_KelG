<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-12">
        <div class="card border-0 rounded shadow">
          <div class="card-body">
            <h4>LOGIN</h4>
            <hr />
            <form @submit.prevent="store">
              <div class="form-group mb-3">
                <label for="content" class="form-label">Email</label>
                <input
                  class="form-control"
                  v-model="user.email"
                  placeholder="Masukkan email"
                />
                <!-- validation -->
                <div v-if="validation.email" class="mt-2 alert alert-danger">
                  {{ validation.email[0] }}
                </div>
              </div>
              <div class="form-group mb-3">
                <label for="content" class="form-label">Password</label>
                <input
                  type="password"
                  class="form-control"
                  v-model="user.password"
                  placeholder="Masukkan password"
                />
                <!-- validation -->
                <div v-if="validation.password" class="mt-2 alert alert-danger">
                  {{ validation.password[0] }}
                </div>
              </div>
              <button type="submit" class="btn btn-primary">Masuk</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
export default {
  setup() {
    //state user
    const user = reactive({
      email: "",
      password: "",
    });
    //state validation
    const validation = ref([]);
    //vue router
    const router = useRouter();
    //method store
    function store() {
      let email = user.email;
      let password = user.password;
      axios
        .post("https://tubesuaswebbackend.itkitapro.com/api/login", {
          email: email,
          password: password,
        })
        .then((response) => {
          localStorage.setItem("id", response.data.user.id);
          localStorage.setItem("name", response.data.user.name);
          localStorage.setItem("email", response.data.user.email);
          localStorage.setItem("token", response.data.access_token);
          //redirect ke post index
          router.push({
            name: "components.home",
          });
        })
        .catch((error) => {
          //assign state validation with error
          validation.value = error.response.user;
        });
    }
    //return
    return {
      user,
      validation,
      router,
      store,
    };
  },
};
</script>