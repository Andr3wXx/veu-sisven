[15:47, 19/5/2024] Alejandro Perea: import 'bootstrap';
import 'bootstrap/dist/css/bootstrap.min.css';

import { library } from '@fortawesome/fontawesome-svg-core';
import {faBank, faBuilding, faPencil, faPhone, faPlus, faTag, faTrash, faUser } from  '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { createApp } from 'vue';

import App from './App.vue';
import router from './router';

library.add(faPhone);
library.add(faUser);
library.add(faTrash);
library.add(faPencil);
library.add(faTag);
library.add(faBuilding);
library.add(faBank);
library.add(faPlus);


createApp(App)
    .component("font-awesome-icon",FontAwesomeIcon)
    .use(router)
    .mount('#app')
[15:49, 19/5/2024] Alejandro Perea: <template>
        <div class="container"> 
            <h1 class="text-start">Listado Categorias |
                <button @click="newCategoria()"
                        class="btn btn-success mx-2">
                        <font-awesome-icon icon="plus" />
                </button>
            </h1>
            <table class="table">
                <thead>
                    <tr>
                        <th scope="col">ID</th>
                        <th scope="col">Nombre</th>
                        <th scope="col">Descripcion</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(categoria, index) in categorias" :key="index">
                        <th scope="row">{{ categoria.id }}</th>
                        <td>{{ categoria.name }}</td>
                        <td>{{ categoria.description }}</td>

                        <td>
                            <button @click="deleteCategoria(categoria.id)"
                                class="btn btn-danger mx-2">
                                <font-awesome-icon icon="trash" />
                            </button>

                            <button @click="editCategoria(categoria.id)"
                                class="btn btn-warning mx-2">
                                <font-awesome-icon icon="pencil" />
                            </button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </template>

    <script>
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: 'Categorias',
        data() {
            return {
                categorias: []
            }
        },
        methods: {
            deleteCategoria(codigo){
                Swal.fire({
                    title: Do you want to delete the Categoria with id ${codigo}?,
                    showCancelButton: true,
                    confirmButtonText: 'Delete',
                }).then((result) =>{
                    if(result.isConfirmed){
                        axios.delete(http://127.0.0.1:8000/api/categorias/${codigo})
                        .then(response =>{
                            if (response.data.success) {
                                Swal.fire('Deleted!!','', 'success');
                                this.categorias = this.categorias.filter(cat => cat.id !== codigo);
                            }
                        })
                    }
                })
            },

            editCategoria(id){
                this.$router.push({name: 'EditarCategoria', params: {id: ${id}}});
            },
            newCategoria(){
                this.$router.push({name: 'NewCategoria'});
            }
        },
        mounted() {
            axios
                .get(http://127.0.0.1:8000/api/categorias)
                .then(response => {
                
                    this.categorias = response.data.categorias;
                })
        },
    }
    </script>