<template>
    <div>
        <Sidebar />
        <div class="main-content">

            <TopNav />

            <div class="container-fluid">
                <div class="row justify-content-center">
                    <div class="col-12 col-lg-10 col-xl-8">

                        <!-- Header -->
                        <div class="header mt-md-5">
                            <div class="header-body">
                                <div class="row align-items-center">
                                    <div class="col">

                                        <!-- Pretitle -->
                                        <h6 class="header-pretitle">
                                            Productos
                                        </h6>

                                        <!-- Title -->
                                        <h1 class="header-title">
                                            Nuevo producto
                                        </h1>

                                    </div>
                                </div> <!-- / .row -->
                                <div class="row align-items-center">
                                    <div class="col">

                                        <!-- Nav -->
                                        <ul class="nav nav-tabs nav-overflow header-tabs">
                                            <li class="nav-item">
                                                <router-link to="/producto" class="nav-link">Todos los productos
                                                </router-link>
                                            </li>
                                            <li class="nav-item">
                                                <a class="nav-link active">Nuevo
                                                    producto</a>
                                            </li>

                                        </ul>

                                    </div>
                                </div>
                            </div>
                        </div>

                        <div class="mb-7">

                            <div class="row justify-content-between align-items-center">
                                <div class="col">
                                    <div class="row align-items-center">
                                        <div class="col-auto">

                                            <!-- Avatar -->
                                            <div class="avatar">
                                                <img class="avatar-img rounded-circle" :src="str_image" alt="...">
                                            </div>

                                        </div>
                                        <div class="col ms-n2">

                                            <!-- Heading -->
                                            <h4 class="mb-1">
                                                <b>Portada</b>
                                            </h4>

                                            <!-- Text -->
                                            <small class="text-muted">
                                                Portada no mayor a 5MB.
                                            </small>

                                        </div>
                                    </div> <!-- / .row -->
                                </div>
                                <div class="col-auto">

                                    <!-- Button -->

                                    <label for="file-upload" class="btn btn-sm btn-primary">
                                        Cargar
                                    </label>
                                    <input style="display:none" id="file-upload" type="file"
                                        v-on:change="uploadImage($event)" />

                                </div>
                            </div> <!-- / .row -->

                            <!-- Divider -->
                            <hr class="my-5">

                            <div class="row">
                                <div class="col-12 col-md-12">

                                    <!-- Email address -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="mb-1">
                                            Título del producto
                                        </label>

                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            Elegir un título adecuado para el producto.
                                        </small>

                                        <!-- Input -->
                                        <input type="text" class="form-control" placeholder="Título del producto"
                                            v-model="producto.titulo">

                                    </div>

                                </div>
                                <div class="col-12 col-md-6">

                                    <!-- First name -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="form-label">
                                            Categoria
                                        </label>
                                        
                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            Seleccionar una categoria para el producto.
                                        </small>

                                        <!-- Input -->
                                        <select name="" class="form-select" v-model="producto.categoria" v-on:change="getSubcategorias($event)">
                                            <option value="" disabled selected>Seleccionar</option>
                                            <option :value="item.categoria.titulo" v-for="item in categorias">{{item.categoria.titulo}}</option>
                                        </select>

                                    </div>

                                </div>
                                <div class="col-12 col-md-6">

                                    <!-- First name -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="form-label">
                                            Sub Categoria
                                        </label>

                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            Seleccionar una sub categoria para el producto.
                                        </small>

                                    <!-- Input -->
                                    <select name="" class="form-select" v-model="producto.subcategoria">
                                        <option value="" disabled selected>Seleccionar</option>
                                        <option :value="item.titulo" v-for="item in subcategorias">{{item.titulo}}</option>
                                    </select>

                                    </div>

                                </div>

                                
                                <div class="col-12 col-md-6">

                                    <!-- Last name -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="form-label">
                                            Variedad
                                        </label>

                                        <!-- Input -->
                                        <input type="text" class="form-control" placeholder="Titulo de la variedad"
                                            v-model="producto.str_variedad">

                                    </div>

                                </div>
                                <div class="col-12 col-md-6">

                                    <!-- Last name -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="form-label">
                                            Precio
                                        </label>

                                        <!-- Input -->
                                        <input disabled type="number" class="form-control" placeholder="Precio" value="0">

                                    </div>

                                </div>

                                <div class="col-12 col-md-12">

                                    <!-- Phone -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="form-label">
                                            Descripción del producto
                                        </label>

                                        <!-- Input -->
                                        <textarea class="form-control" id="" rows="3" placeholder=""
                                            v-model="producto.extracto"></textarea>

                                    </div>

                                </div>

                            </div> <!-- / .row -->



                            <div class="row">
                                <div class="col-12 col-md-6">

                                    <!-- Public profile -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="mb-1">
                                            Producto publicado
                                        </label>

                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            No publicar un producto recien creado.
                                        </small>

                                        <div class="row">
                                            <div class="col-auto">

                                                <!-- Switch -->
                                                <div class="form-check form-switch">
                                                    <input class="form-check-input" type="checkbox" id="switchOne"
                                                        v-model="producto.estado" />
                                                    <label class="form-check-label" for="switchOne"></label>
                                                </div>

                                            </div>
                                            <div class="col ms-n2">

                                                <!-- Help text -->
                                                <small class="text-muted">
                                                    Borrador activado
                                                </small>

                                            </div>
                                        </div> <!-- / .row -->
                                    </div>

                                </div>
                                <div class="col-12 col-md-6">

                                    <!-- Allow for additional Bookings -->
                                    <div class="form-group">

                                        <!-- Label -->
                                        <label class="mb-1">
                                            En descuento
                                        </label>

                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            El descuento solo estaré en base a la necesidad del producto y cliente.
                                        </small>

                                        <div class="row">
                                            <div class="col-auto">

                                                <!-- Switch -->
                                                <div class="form-check form-switch">
                                                    <input class="form-check-input" type="checkbox" id="switchTwo"
                                                        v-model="producto.descuento" />
                                                    <label class="form-check-label" for="switchTwo"></label>
                                                </div>

                                            </div>
                                            <div class="col ms-n2">

                                                <!-- Help text -->
                                                <small class="text-muted">
                                                    Descuento desactivado
                                                </small>

                                            </div>
                                        </div> <!-- / .row -->
                                    </div>

                                </div>
                            </div> <!-- / .row -->

                            <!-- Divider -->
                            <hr class="mt-3 mb-5">

                            <!-- Button -->
                            <button class="btn btn-primary" v-on:click="validad()">
                                Crear producto
                            </button>


                        </div>

                    </div>
                </div> <!-- / .row -->
            </div>

        </div>
    </div>
</template>
    
<script>

import Sidebar from '@/components/Sidebar.vue';
import TopNav from '@/components/TopNav.vue';
import axios from 'axios';

export default {
    name: 'CreateDevolucionApp',
    components: {
        Sidebar,
        TopNav
    },
    data() {
        return {
            str_image: '/assets/img/avatar-1.jpg',
            producto: {
                categoria: '',
                estado: false,
                descuento: false,
                portada: undefined,
                subcategoria: ''
            },
            categorias: [],
            subcategorias: [],
            portada: undefined,
        }
    },
    beforeMount(){
        this.init_categorias();
    },
    methods: {
        uploadImage($event) {

            var image;

            if ($event.target.files.length >= 1) {
                image = $event.target.files[0];
            }

            if (image.size <= 2000000) {
                if (image.type == 'image/jpeg' || image.type == 'image/png' || image.type == 'image/webp' || image.type == 'image/jpg') {
                    this.str_image = URL.createObjectURL(image);
                    this.portada = image;
                    this.producto.portada = this.portada;
                } else {
                    this.$notify({
                        group: 'foo',
                        title: 'ERROR',
                        text: 'El recurso debe ser una imagen.',
                        type: 'error'
                    });
                    this.portada = undefined;
                }
            } else {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'La imagen debe pesar menos de 5MB.',
                    type: 'error'
                });
                this.portada = undefined;
            }
        },

        validad() {
            if (!this.producto.titulo) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el título del producto.',
                    type: 'error'
                });
            } else if (!this.producto.categoria) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Seleccione la categoría del producto.',
                    type: 'error'
                });
            } else if (!this.producto.subcategoria) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Seleccione la subcategoria del producto.',
                    type: 'error'
                });
            } else if (!this.producto.extracto) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el extracto del producto',
                    type: 'error'
                });
            } else if (!this.producto.str_variedad) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese la variedad del producto.',
                    type: 'error'
                });
            } else if (this.producto.portada == undefined) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Seleccione una imagen de portada.',
                    type: 'error'
                });
            } else {
                this.registro();
            }
        },

        registro() {
            var fm = new FormData();

            fm.append('titulo', this.producto.titulo);
            fm.append('categoria', this.producto.categoria);
            fm.append('subcategoria', this.producto.subcategoria);
            fm.append('extracto', this.producto.extracto);
            fm.append('estado', this.producto.estado);
            fm.append('str_variedad', this.producto.str_variedad);
            fm.append('descuento', this.producto.descuento);
            fm.append('portada', this.producto.portada);

            axios.post(this.$url + '/registro_producto_admin', fm, {
                headers: {
                    'Content-Type': 'multipart/form-data',
                    'Authorization': this.$store.state.token
                }
            }).then((result) => {
                if (result.data.message) {
                    this.$notify({
                        group: 'foo',
                        title: 'ERROR',
                        text: result.data.message,
                        type: 'error'
                    });
                } else {
                    this.$notify({
                        group: 'foo',
                        title: 'SUCCESS',
                        text: 'Se registró correctamente el producto.',
                        type: 'success'
                    });

                    this.$router.push({ name: 'producto-index' });
                }
            })

        },
        init_categorias(){
          axios.get(this.$url+'/listar_categorias_admin',{
              headers:{
                  'Content-Type': 'application/json',
                  'Authorization': this.$store.state.token,
              }
          }).then((result)=>{
              this.categorias = result.data;
          });
      },
      getSubcategorias(event){
            for(var item of this.categorias){
                if(item.categoria.titulo == event.target.value){
                    this.subcategorias = item.subcategorias;
                }
            }
      },
    },
}
</script>
    