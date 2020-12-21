<template>
  <div>
    <!-- formulario de logeo -->
    <div class="container-fluid bg">
      <div class="row justify-content-center">
        <section class="col-12 col-sm-6 col-md-3">
          <form class="form-container">
            <div class="form-group">
              <label for="exampleInputEmail1" class="form-label"
                >Correo Electrónico</label
              >
              <input
                type="email"
                class="form-control"
                id="exampleInputEmail1"
                aria-describedby="emailHelp"
                v-model="login.email"
              />
            </div>

            <div class="form-group">
              <label for="exampleInputPassword1" class="form-label"
                >Contraseña</label
              >
              <input
                type="password"
                class="form-control"
                id="exampleInputPassword1"
                v-model="login.password"
              />
            </div>

            <button
              type="submit"
              class="btn btn-primary btn-block"
              @click.prevent="loginUser"
            >
              Iniciar Sesión
            </button>
            <div class="mt-2 text-center">
              <a href="#"> Olvidaste tu contraseña? </a>
            </div>
          </form>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
// importo sweetalert
import swal from "sweetalert";
import VueJwtDecode from 'vue-jwt-decode'

export default {
  data() {
    return {
      login: {
        email: "",
        password: "",
      },
    };
  },
  methods: {
    async loginUser() {
      try {
        //console.log(this.login);
        let response = await this.$http.post("/api/usuario/login", this.login);
        console.log(response.data);
        let token = response.data.tokenReturn;
        let user = response.data.user;

        localStorage.setItem("jwt", token);
        localStorage.setItem("user", JSON.stringify(user));

        if (token) {
          swal("Bienvenido!", "Login correcto", "success");
          this.$router.push("/home");
        }
      } catch (error) {
        swal("Oops!", "Algo salió mal", "error");
      }
    },
  },
};
</script>

<style scoped>
.bg {
  background: url("../../assets/bg1.jpg");
  width: 100%;
  height: 100vh;
  background-size: 100%;
}
.form-container {
  position: absolute;
  top: 15vh;
  background: #fff;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 0px 10px #000;
}
</style>