<template>
  <v-app id="inspire">
    
    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Group 37</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn
      icon
      class="mr-5"
      @click = "salir()">
        <v-icon>mdi-logout</v-icon>
        <span>salir</span>
      </v-btn>

    </v-app-bar>

    <!-- Menú desplegable del lato izquierdo -->
    <v-navigation-drawer v-model="drawer" fixed temporary>
      <v-card class="mx-auto" width="300">
        <v-list>
          <v-list-item :to="{ name: 'Home' }">
            <v-list-item-icon>
              <v-icon>mdi-home</v-icon>
            </v-list-item-icon>

            <v-list-item-title>Inicio</v-list-item-title>
          </v-list-item>

          <v-list-group :value="true" prepend-icon="mdi-cube-outline">
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Administrar</v-list-item-title>
              </v-list-item-content>
            </template>

            <v-list-item
              v-for="([title, icon, ruta], i) in admins"
              :key="i"
              :to="{ name: ruta }"
              exact
            >
              <v-list-item-title v-text="title"></v-list-item-title>

              <v-list-item-icon>
                <v-icon v-text="icon"></v-icon>
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>

          <v-list-group :value="true" prepend-icon="mdi-account-check-outline">
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Permisos</v-list-item-title>
              </v-list-item-content>
            </template>

            <v-list-item v-for="([title, icon, ruta], i) in cruds" 
            :key="i" 
            exact
            :to="{ name: ruta }">
              <v-list-item-title v-text="title"></v-list-item-title>

              <v-list-item-icon>
                <v-icon v-text="icon"></v-icon>
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>
        </v-list>
      </v-card>
    </v-navigation-drawer>
    <!-- Aquí termina el menú desplegable del lado izquierdo -->

    <v-main class="grey lighten-2">
      <v-container>
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template> 

<script>


export default {
  name: "SeguraComponent",

  components: {
    
  },
  data: () => ({
    //Script del wireframe
    drawer: null,

    // Scripts de las listas desplegables
    admins: [
      ["Categorias", "mdi-account-multiple-outline", "Categoria"],
      ["Articulos", "mdi-cog-outline", "Articulos"],
    ],
    cruds: [
      ["Usuarios", "mdi-plus-outline", "Usuario"],
    ],
    //Aqui terminan los Scripts de las listas desplegables
  }),
  created(){
    this.$store.dispatch('autoLogin');
  },

  methods:{
    salir(){
      this.$store.dispatch('salir');
    }
  }
};
</script>
