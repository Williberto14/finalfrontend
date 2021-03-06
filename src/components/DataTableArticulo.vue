<template>
<div id="app">
  <v-app id="inspire">
    <v-data-table 
    :headers="headers" 
    :items="articulos" 
    sort-by="id" 
    class="elevation-1"
    :loading="cargando"
    loading-text="Cargando... Espere por favor"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Artículos</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                Agregar Artículo
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
                      <v-textarea 
                      v-model="editedItem.descripcion" 
                      label="Descripción"
                      auto-grow
                      no-resize
                      counter="250"
                      ></v-textarea>
                    </v-col>

                     <v-col cols="12">
                      <v-select 
                      v-model="categoria" 
                      label="Categoria"
                      :items="categorias"
                      item-text="nombre"
                      item-value="id"
                      return-object
                      ></v-select>
                    </v-col>

                     <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.codigo" 
                      label="Código"
                      ></v-text-field>
                    </v-col>

                     <v-col cols="12">
                      <v-text-field 
                      v-model="editedItem.estado" 
                      label="Estado"
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
              <v-card-title class="headline">Está seguro de cambiar el estado de este artículo?</v-card-title>
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
        value: 'nombre',
      },
      { text: 'Descripción', value: 'descripcion' },
      { text: 'Categoria', value: 'categoria.nombre' },
      { text: 'Código', value: 'codigo' },
      { text: 'Estado', value: 'estado' },
      { text: 'Actions', value: 'actions', sortable: false },
    ],
    desserts: [],

// Creo este objeto que equivale a lo que me devolveria la peticion al backend para hacer pruebas
    articulos: [],
    categorias: [],
    categoria: '',

    editedIndex: -1,
    editedItem: {
      id: 0,
      nombre: '',
      descripcion: '',
      codigo:'',
      estado: 0,
      categoria:{
        id:0,
        nombre:'',
      }
    },
    defaultItem: {
      id: 0,
      nombre: '',
      descripcion: '',
      codigo:'',
      estado: 0,
      categoria:{
        id:0,
        nombre:'',
      }
    }
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'Nuevo artículo' : 'Editar artículo'
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
    this.listCategorias();
  },

  methods: {

    //Metodo para hacer consulta ala base de datos 
    list(){
      axios.get('http://localhost:3000/api/articulo/list')
      .then( response =>{
        this.articulos = response.data;
        this.cargando = false;
      })
      .catch(error =>{
        console.log(error);
      })
    },
        listCategorias(){
      axios.get('http://localhost:3000/api/categoria/list')
      .then( response =>{
        this.categorias = response.data;
      })
      .catch(error =>{
        console.log(error);
      })
    },

    editItem (item) {
      this.editedIndex = item.id
      this.categoria = item ? item.categoria: '';
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
        axios.put('http://localhost:3000/api/articulo/deactivate', {
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
        axios.put('http://localhost:3000/api/articulo/activate', {
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
        this.categoria = ''
        this.editedIndex = -1
      })
    },
    

    save () {
      if (this.editedIndex > -1) {
        //put
        axios.put('http://localhost:3000/api/articulo/update', {
          "id": this.editedItem.id,
          "nombre": this.editedItem.nombre,
          "descripcion": this.editedItem.descripcion,
        "codigo": this.editedItem.codigo,
        "categoria": this.categoria.id,
        })
        .then( response =>{
          this.list();
        })
        .catch(error => {
          return error;
        })
      } else {
        //post
        axios.post('http://localhost:3000/api/articulo/add', {
          "estado": 1,
          "nombre": this.editedItem.nombre,
          "descripcion": this.editedItem.descripcion,
          "codigo": this.editedItem.codigo,
          "categoriaId": this.categoria.id,
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