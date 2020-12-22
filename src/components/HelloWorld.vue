<template>
  <div>
    <header>
      <!-- Barra de navegación  -->
      <nav class="navbar navbar-expand-lg navbar-light bg-dark">
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item">
              <a class="btn btn-outline-light mr-2" href="#serviser">Servicios</a>
            </li>
            <li class="nav-item">
              <a class="btn btn-outline-light mr-2" href="#historias">Historias</a>
            </li>
            <li class="nav-item">
              <a class="btn btn-outline-light" href="#download">Quiénes somos</a>
            </li>
          </ul>
          <form class="form-inline my-2 my-lg-0">
            <button 
            @click.prevent="toLogin" 
            class="btn btn-primary my-2 my-sm-0">
              Iniciar sesión
            </button>
          </form>
        </div>
      </nav>

      <!-- Carrusel -->
      <template>
        <v-carousel>
          <v-carousel-item
            v-for="(item, i) in items"
            :key="i"
            :src="item.src"
            reverse-transition="fade-transition"
            transition="fade-transition"
          ></v-carousel-item>
        </v-carousel>
      </template>
    </header>

    <main>
      <!-- Descripcion general de la empresa -->
      <section id="download">
        <v-container fluid>
          <v-row align="center" justify="center">
            <v-col cols="10">
              <v-row align="center" justify="center">
                <v-col sm="4" class="hidden-xs-only">
                  <v-img
                    src="../assets/c7.png"
                    class="d-block ml-auto mr-auto"
                    max-width="350px"
                  />
                </v-col>
                <v-col cols="12" sm="8" class="white--text text-left">
                  <h1 class="font-weight-light display-2 mb-2">
                    A que nos dedicamos?
                  </h1>
                  <h1 class="font-weight-light">
                    Estamos totalmente enfocados en ayudar a las organizaciones en su gestión de la 
                    sostenibilidad, aportando servicios y soluciones que crean valor y beneficio económico, 
                    social y medioambiental.
                  </h1>
                  
                </v-col>
              </v-row>
            </v-col>
          </v-row>
        </v-container>
      </section>





      <!-- Sección de servicios -->
      <section id="serviser">
        <div class="container-fluid">
          <div class="row justify-content-center mb-5">

              <div class="col-md-4 card text-white bg-dark tamano">
                <div class="d-flex justify-content-center p-2">
                  <img src="../assets/l1.jpg" />
                </div>
                <div class="card-body">         
                  <h5 class="card-title">{{ servicios[1].nombre }}</h5>
                  <p class="card-text">{{ servicios[1].descripcion }}</p>
                </div>
              </div>

              <div class="col-md-4 card text-white bg-dark tamano">
                <div class="d-flex justify-content-center p-2">
                  <img src="../assets/l2.jpg" />
                </div>
                <div class="card-body">         
                  <h5 class="card-title">{{ servicios[1].nombre }}</h5>
                  <p class="card-text">{{ servicios[1].descripcion }}</p>
                </div>
              </div>

              <div class="col-md-4 card text-white bg-dark tamano">
                <div class="d-flex justify-content-center p-2">
                  <img src="../assets/l3.jpg" />
                </div>
                <div class="card-body">         
                  <h5 class="card-title">{{ servicios[2].nombre }}</h5>
                  <p class="card-text">{{ servicios[2].descripcion }}</p>
                </div>
              </div>
          </div>
        </div>
      </section>
       <!-- <section id="serviser">
        <div class="container-fluid">
          <div class="row justify-content-center mb-5">
            <div v-for="servicio in servicios" :key="servicio.codigo" class="col mt-5">
              <div class="card text-white bg-dark tamano">
                <div class="d-flex justify-content-center p-2">
                  <img src="https://placeimg.com/200/150/any" />
                </div>
                <div class="card-body">         
                  <h5 class="card-title">{{ team[2].Nombre }}</h5>
                  <p class="card-text">{{ servicio.descripcion }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section> -->





      <section id="historias">
        <div class="card-group">
          <div class="card">
            <img
              src="../assets/p1.jpg"
              class="card-img-top"
              
              alt="..."
            />
            <div class="card-body">
              <h5 class="card-title text-success">Ricardo Goez - Cirujano plastico</h5>
              <p class="card-text text-dark">
                El reconocido cirujano contrató nuestros servicios con una iniciativa altruista
                 para la sociedad esto aumento sus clientes en más de un 40%
              </p>
              <p class="card-text">
                <small class="text-muted">Asistido en 2012</small>
              </p>
            </div>
          </div>
          <div class="card">
            <img
              src="../assets/p2.jpg"
              class="card-img-top"
             
              alt="..."
            />
            <div class="card-body">
              <h5 class="card-title text-success">Muebles Gonzales</h5>
              <p class="card-text text-dark">
                Esta empresa en una hermosa iniciativa de brindar a las pequeñas escuelas del departamento
                 de sucre nuevos pupitres. A partir de este momento sus ventas no han parado de subir.
              </p>
              <p class="card-text">
                <small class="text-muted">Asistido en 2018</small>
              </p>
            </div>
          </div>
          <div class="card">
            <img
              src="../assets/p3.jpg"
              class="card-img-top"
              alt="..."
            />
            <div class="card-body">
              <h5 class="card-title text-success">Transportista Express</h5>
              <p class="card-text text-dark">
                Solicito nuestro apoyo para enfocar su compromiso con la ciudadanía, se ha convertido
                 en la empresa preferida en la región caribe gracias a sus grandes aportes en áreas
                  como el deporte y la educación
              </p>
              <p class="card-text">
                <small class="text-muted">Asistido en 2015</small>
              </p>
            </div>
          </div>
        </div>
      </section>
    </main>

    <footer>
      <v-footer dark padless>
        <v-card flat tile class="indigo lighten-1 white--text text-center">
          <!-- <v-card-text>
            <v-btn
              v-for="icon in icons"
              :key="icon"
              class="mx-4 white--text"
              icon
            >
              <v-icon size="24px">
                {{ icon }}
              </v-icon>
            </v-btn>
          </v-card-text> -->

          <v-card-text class="white--text pt-0 mt-5">
            Operamos en todas las capitales  del territorio nacional colombiano, con nuestra sede principal en la ciudad se Bogotá, además de  tener mucha presencia ciudades como Medellín, Cali, Montería y Bucaramanga     <br> 
            Información de contacto: <br>
            resolutions@miempresaresponsable.com  || servicioalcliente@miempresaresponsable.com <br>
            018000124578 - 312-4578412 

          </v-card-text>

          <v-divider></v-divider>
            <v-btn
            rounded
            outlined
            href="https://github.com/Williberto14/finalfrontend.git"
            target="_blank"
            large
            color="white"
            class="mt-4 mr-3"
            >
            <v-icon class="mr-2"> mdi-github </v-icon>
            Git Hub Frontend
            </v-btn>

            <v-btn
            rounded
            outlined
            href="https://github.com/Worckshop37/semana-5-37.git"
            target="_blank"
            large
            color="white"
            class="mt-4"
            >
            <v-icon class="mr-2"> mdi-github </v-icon>
            Git Hub Backend
            </v-btn>
          <v-card-text class="white--text">
            {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
          </v-card-text>
        </v-card>
      </v-footer>
    </footer>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",

  data() {
    return {
      // icons: ["mdi-facebook", "mdi-twitter", "mdi-linkedin", "mdi-instagram"],
      servicios : [],
      items: [
        {
          src: require('../assets/c4.png'),
        },
        {
          src: require('../assets/c2.jpg'),
        },
        {
          src: require('../assets/c3.jpg'),
        },
        {
          src: require('../assets/c5.jpg'),
        },
      ],

      datos: null,
      team: [
        {
          codigo: 1,
          Nombre: "Lucas Mera",
          Descripcion:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris condimentum ac elit et accumsan.",
          Rol: "Desarrollador backend",
          Color: "bg-danger",
        },
        {
          codigo: 2,
          Nombre: "Cristian Sanz",
          Descripcion:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris condimentum ac elit et accumsan.",
          Rol: "Arquictecto de software",
          Color: "bg-dark",
        },
        {
          codigo: 3,
          Nombre: "Samara Cerezo",
          Descripcion:
            "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris condimentum ac elit et accumsan.",
          Rol: "Tester",
          Color: "bg-danger",
        },
      ],
    };
  },
  created(){
    this.listServices();
  },
  methods: {
    toLogin(){
      this.$router.push({name: 'Login'});
    },
    listServices(){
      axios.get('http://localhost:3000/api/articulo/list')
      .then( response =>{
        this.servicios = response.data;
      })
      .catch(error =>{
        console.log(error);
      })
    },
  }
};
</script>

<style scoped>
.tamano {
  /* width: 350px; */
  height: 450px;
}

#download {
  background-image: url("~@/assets/img/bgDownload.jpg");
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  height: 500px;
}

#download .container,
#download .row {
  height: 100%;
}
</style>
