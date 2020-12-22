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
            <button class="btn btn-primary my-2 my-sm-0" type="button">
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
                    src="@/assets/img/ill2.svg"
                    class="d-block ml-auto mr-auto"
                    max-width="350px"
                  />
                </v-col>
                <v-col cols="12" sm="8" class="white--text text-left">
                  <h1 class="font-weight-light display-2 mb-2">
                    Baixar Demonstração
                  </h1>
                  <h1 class="font-weight-light">
                    Lorem ipsum dolor sit amet consectetur adipisicing elit.
                    Iste ex animi quod laboriosam vel blanditiis labore alias,
                    aliquid, tempora repellendus non.
                  </h1>
                  <v-btn
                    rounded
                    outlined
                    href="https://github.com/Joabsonlg/Landing-Page"
                    target="_blank"
                    large
                    color="white"
                    class="mt-4"
                  >
                    <v-icon class="mr-2"> mdi-github </v-icon>
                    Git Hub
                  </v-btn>
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
            <div v-for="miembro in team" :key="miembro.codigo" class="col mt-5">
              <div class="card text-white bg-dark tamano">
                <div class="d-flex justify-content-center p-2">
                  <img src="https://placeimg.com/200/150/any" />
                </div>
                <div class="card-body">
                  <h5 class="card-title">
                    Team member: {{ miembro.codigo }} {{ miembro.Rol }}
                  </h5>
                  <h5 class="card-title">{{ miembro.Nombre }}</h5>
                  <p class="card-text">{{ miembro.Descripcion }}</p>
                </div>
                <div class="card-footer">
                  <small class="text-muted">Last updated 3 mins ago</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="historias">
        <div class="card-group">
          <div class="card">
            <img
              src="https://placeimg.com/300/300/any"
              class="card-img-top"
              alt="..."
            />
            <div class="card-body">
              <h5 class="card-title text-success">Card title</h5>
              <p class="card-text text-dark">
                This is a wider card with supporting text below as a natural
                lead-in to additional content. This content is a little bit
                longer.
              </p>
              <p class="card-text">
                <small class="text-muted">Last updated 3 mins ago</small>
              </p>
            </div>
          </div>
          <div class="card">
            <img
              src="https://placeimg.com/300/300/any"
              class="card-img-top"
              alt="..."
            />
            <div class="card-body">
              <h5 class="card-title text-success">Card title</h5>
              <p class="card-text text-dark">
                This card has supporting text below as a natural lead-in to
                additional content.
              </p>
              <p class="card-text">
                <small class="text-muted">Last updated 3 mins ago</small>
              </p>
            </div>
          </div>
          <div class="card">
            <img
              src="https://placeimg.com/300/300/any"
              class="card-img-top"
              alt="..."
            />
            <div class="card-body">
              <h5 class="card-title text-success">Card title</h5>
              <p class="card-text text-dark">
                This is a wider card with supporting text below as a natural
                lead-in to additional content. This card has even longer content
                than the first to show that equal height action.
              </p>
              <p class="card-text">
                <small class="text-muted">Last updated 3 mins ago</small>
              </p>
            </div>
          </div>
        </div>
      </section>
    </main>

    <footer>
      <v-footer dark padless>
        <v-card flat tile class="indigo lighten-1 white--text text-center">
          <v-card-text>
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
          </v-card-text>

          <v-card-text class="white--text pt-0">
            Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit amet.
            Mauris cursus commodo interdum. Praesent ut risus eget metus luctus
            accumsan id ultrices nunc. Sed at orci sed massa consectetur
            dignissim a sit amet dui. Duis commodo vitae velit et faucibus.
            Morbi vehicula lacinia malesuada. Nulla placerat augue vel ipsum
            ultrices, cursus iaculis dui sollicitudin. Vestibulum eu ipsum vel
            diam elementum tempor vel ut orci. Orci varius natoque penatibus et
            magnis dis parturient montes, nascetur ridiculus mus.
          </v-card-text>

          <v-divider></v-divider>

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
      icons: ["mdi-facebook", "mdi-twitter", "mdi-linkedin", "mdi-instagram"],

      items: [
        {
          src: "https://cdn.vuetifyjs.com/images/carousel/squirrel.jpg",
        },
        {
          src: "https://cdn.vuetifyjs.com/images/carousel/sky.jpg",
        },
        {
          src: "https://cdn.vuetifyjs.com/images/carousel/bird.jpg",
        },
        {
          src: "https://cdn.vuetifyjs.com/images/carousel/planet.jpg",
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
};
</script>

<style scoped>
.tamano {
  width: 350px;
  height: 400px;
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
