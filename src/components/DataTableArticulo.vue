<template>
<div id="app">
  <v-app id="inspire">
    <v-data-table 
    :headers="headers" 
    :items="articulos" 
    sort-by="nombre" 
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
                  Cancel
                </v-btn>
                <v-btn color="blue darken-1" text @click="save">
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
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
    cargando: false, //al conectar a la base de datos este quedara true
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
    articulos: [
      {
        "id": 1,
        "codigo": "12",
        "nombre": "articulo 1",
        "descripcion": "soy un articulo 1",
        "estado": 1,
        "categoriaId": 1,
        "createdAt": "30/01/1991",
        "updateAt": "05/02/1951",
        "categoria": {
          "id": 1,
          "nombre": "categoria 1",
          "descripcion": "Soy una categoria numero 1",
          "estado": 1,
          "createdAt": "30/01/1891",
          "updateAt": "05/02/1851"
        }
      },
       {
        "id": 2,
        "codigo": "13",
        "nombre": "articulo 2",
        "descripcion": "soy un articulo 2",
        "estado": 1,
        "categoriaId": 2,
        "createdAt": "30/01/1991",
        "updateAt": "05/02/1951",
        "categoria": {
          "id": 1,
          "nombre": "categoria 1",
          "descripcion": "Soy una categoria numero 1",
          "estado": 1,
          "createdAt": "30/01/1891",
          "updateAt": "05/02/1851"
          }
        },
        {
        "id": 3,
        "codigo": "14",
        "nombre": "articulo 3",
        "descripcion": "soy un articulo 3",
        "estado": 0,
        "categoriaId": 3,
        "createdAt": "30/01/1991",
        "updateAt": "05/02/1951",
        "categoria": {
          "id": 2,
          "nombre": "categoria 2",
          "descripcion": "Soy una categoria numero 2",
          "estado": 1,
          "createdAt": "30/01/1891",
          "updateAt": "05/02/1851"
          }
        },
    ],
    categorias: [
      {
        "id": 1,
        "nombre": "categoria 1",
        "descripcion": "soy una categoria 1",
        "estado": 1,
        "createdAt": "30/01/1991",
        "updateAt": "05/02/1951"
      },
      {
        "id": 2,
        "nombre": "categoria 2",
        "descripcion": "soy una categoria 2",
        "estado": 1,
        "createdAt": "31/01/1991",
        "updateAt": "06/02/1951"
      },
      {
        "id": 3,
        "nombre": "categoria 3",
        "descripcion": "soy una categoria 3",
        "estado": 0,
        "createdAt": "31/02/1991",
        "updateAt": "06/03/1951"
      }
    ],

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
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
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
    //this.list(); para enlazar con el backend al activar ese se borra el de abajo
    //this.listCategorias(); para enlazar con el backend al activar ese se borra el de abajo
    this.initialize()
  },

  methods: {
    initialize () {
      this.desserts = [
        {
          nombre: 'Frozen Yogurt',
          descripcion: 159,
          estado: 6.0,
        },
      ]
    },
    //Metodo para hacer consulta ala base de datos 
    // list(){
    //   axios.get('http://localhost:3000/api/articulo/list')
    //   .then( response =>{
    //     this.articulos = response.data;
    //     this.cargando = false;
    //   })
    //   .catch(error =>{
    //     console.log(error);
    //   })
    // },
        // listCategorias(){
    //   axios.get('http://localhost:3000/api/categoria/list')
    //   .then( response =>{
    //     this.categorias = response.data;
    //   })
    //   .catch(error =>{
    //     console.log(error);
    //   })
    // },

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

    deleteItemConfirm () {
          
      if (this.editedItem === 1) {
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
        // axios.put('http://localhost:3000/api/articulo/update', {
        //   "id": this.editedItem.id,
        //   "nombre": this.editedItem.nombre,
        //   "descripcion": this.editedItem.descripcion,
        // "codigo": this.editedItem.codigo,
        // "categoria": this.categoria.id,
        // })
        // .then( response =>{
        //   this.list();
        // })
        // .catch(error => {
        //   return error;
        // })
        Object.assign(this.desserts[this.editedIndex], this.editedItem)//cuando se active lo de arriba este se borra
      } else {
        //post
        // axios.post('http://localhost:3000/api/articulo/add', {
        //   "estado": 1,
        //   "nombre": this.editedItem.nombre,
        //   "descripcion": this.editedItem.descripcion,
        //   "codigo": this.editedItem.codigo,
        //   "categoriaId": this.categoria.id,
        // })
        // .then( response =>{
        //   this.list();
        // })
        // .catch(error => {
        //   return error;
        // })

        this.desserts.push(this.editedItem)//esta tambien se borra al activar lo de arriba
      }
      this.close()
    },
  },
}
</script>