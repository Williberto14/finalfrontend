<template>
<div id="app">
  <v-app id="inspire">
    <v-data-table 
    :headers="headers" 
    :items="usuarios" 
    sort-by="id" 
    class="elevation-1"
    :loading="cargando"
    loading-text="Cargando... Espere por favor"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Usuarios</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                Agregar Usuario
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>

                    <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.id" 
                      label="ID"
                      ></v-text-field>
                    </v-col>
                    
                    <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.nombre" 
                      label="Nombre"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.email" 
                      label="Correo"
                      ></v-text-field>
                    </v-col>

                    <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.estado" 
                      label="Estado"
                      ></v-text-field>
                    </v-col>

                     <v-select 
                      v-model="editedItem.rol" 
                      label="Rol"
                      :items="roles"
                      return-object
                      ></v-select>

                    <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.password" 
                      label="Contraseña"
                      ></v-text-field>
                    </v-col>
                    
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">
                  Cancelar
                </v-btn>
                <v-btn color="blue darken-1" text @click="save">
                  Guardar
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="600px">
            <v-card>
              <v-card-title class="headline">Está seguro de cambiar el estado de este usuario?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
                <v-btn color="blue darken-1" text @click="changeState">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>

      <template v-slot:item.actions="{ item }">

        <v-icon 
        small class="mr-2"
        @click="editItem(item)"
        >
        mdi-pencil
        </v-icon>

        <v-icon 
        medium 
        @click="deleteItem(item)"
        >
        <!-- Programa boton switch para que encienda y apague al cambiar el estado  -->
        <template v-if="item.estado">
        mdi-toggle-switch
        </template>
        <template v-else>
        mdi-toggle-switch-off-outline
        </template>
        </v-icon>

      </template>

      <template v-slot:no-data>
        <v-btn color="primary" @click="initialize">
          Reset
        </v-btn>
      </template>
    </v-data-table>
  </v-app>
</div>
</template>

<script>

import axios from 'axios';

export default {
    data: () => ({
    dialog: false,
    dialogDelete: false,
    cargando: true, //al conectar a la base de datos este quedara true
    headers: [
      { text: 'ID', value: 'id' },
      {
        text: 'Nombre',
        align: 'start',
        sortable: true,
        value: 'nombre',//Aqui puede haber un error revisar como se llama ento en la base de datos
      },
      { text: 'Correo', value: 'email' },
      { text: 'Rol', value: 'rol' },
      { text: 'Estado', value: 'estado' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
    desserts: [],
    roles: ["Administrador", "Vendedor", "Almacenero"],

    usuarios: [],
    editedIndex: -1,
    editedItem: {
      id:0,
      nombre: '',
      rol: '',
      email: '',
      password:'',
      estado: 0,
    },
    defaultItem: {
      id:0,
      nombre: '',
      rol: '',
      email: '',
      password:'',
      estado: 0,
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'Nuevo usuario' : 'Editar usuario'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  created () {
    this.list();
  },

  methods: {
    
    //Metodo para hacer consulta ala base de datos 
    list(){
      axios.get('http://localhost:3000/api/usuario/list')
      .then( response =>{
        this.usuarios = response.data;
        this.cargando = false;
      })
      .catch(error =>{
        console.log(error);
      })
    },

    editItem (item) {
      this.editedIndex = item.id
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    changeState () {
          
      if (this.editedItem.estado === 1) {
        //put
        axios.put('http://localhost:3000/api/usuario/deactivate', {
          "id": this.editedItem.id,
        })
        .then( response =>{
          this.list();
        })
        .catch(error => {
          return error;
        })
      } else {
        //post
        axios.put('http://localhost:3000/api/usuario/activate', {
          "id": this.editedItem.id,
        })
        .then( response =>{
          this.list();
        })
        .catch(error => {
          return error;
        })
      }

      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        //put
        axios.put('http://localhost:3000/api/usuario/update', {
         "rol": this.editedItem.rol,
          "nombre": this.editedItem.nombre,
          "password": this.editedItem.password,
          "email": this.editedItem.email,
        })
        .then( response =>{
          this.list();
        })
        .catch(error => {
          return error;
        })
      } else {
        //post
        axios.post('http://localhost:3000/api/categoria/add', {
          "rol": this.editedItem.rol,
          "nombre": this.editedItem.nombre,
          "password": this.editedItem.password,
          "email": this.editedItem.email
        })
        .then( response =>{
          this.list();
        })
        .catch(error => {
          return error;
        })

      }
      this.close()
    },
  },
}
</script>