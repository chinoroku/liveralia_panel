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
                                            Proveedores
                                        </h6>

                                        <!-- Title -->
                                        <h1 class="header-title">
                                            Nuevo proveedor
                                        </h1>

                                    </div>
                                </div> <!-- / .row -->
                                <div class="row align-items-center">
                                    <div class="col">

                                        <!-- Nav -->
                                        <ul class="nav nav-tabs nav-overflow header-tabs">
                                            <li class="nav-item">
                                                <router-link to="/proveedor" class="nav-link">Todos los
                                                    proveedores</router-link>
                                            </li>
                                            <li class="nav-item">
                                                <a class="nav-link active">
                                                    Nuevo proveedor
                                                </a>

                                            </li>

                                        </ul>

                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Form -->


                        <div class="row">
                            <div class="col-12 col-md-6">

                                <!-- First name -->
                                <div class="form-group">

                                    <!-- Label -->
                                    <label class="form-label">
                                        Nombre
                                    </label>

                                    <!-- Input -->
                                    <input type="text" class="form-control" v-model="proveedor.nombre"
                                        placeholder="Nombre proveedor">

                                </div>

                            </div>
                            <div class="col-12 col-md-6">

                                <!-- Last name -->
                                <div class="form-group">

                                    <!-- Label -->
                                    <label class="form-label">
                                        Correo electrónico
                                    </label>

                                    <!-- Input -->
                                    <input type="text" class="form-control" v-model="proveedor.email"
                                        placeholder="Correo electrónico">

                                </div>

                            </div>
                            <div class="col-12 col-md-6">

                                <!-- Email address -->
                                <div class="form-group">

                                    <!-- Label -->
                                    <label class="mb-1">
                                        Celular
                                    </label>

                                    <!-- Form text -->
                                    <small class="form-text text-muted">
                                        Poner un celular válido para las comunicaciones respectivas.
                                    </small>

                                    <!-- Input -->
                                    <input type="celular" class="form-control" v-model="proveedor.celular"
                                        placeholder="Celular">

                                </div>

                            </div>

                        </div> <!-- / .row -->

                        <!-- Divider -->

                        <hr class="my-3">

                        <!-- Button -->
                        <button type="button" class="btn btn-primary" v-on:click="validar()">
                            Guardar colaborador
                        </button>



                        <br><br>

                    </div>
                </div> <!-- / .row -->
            </div>

        </div>
    </div>
</template>

<script>
// @ is an alias to /src
import Sidebar from '@/components/Sidebar.vue';
import TopNav from '@/components/TopNav.vue';
import axios from 'axios';

export default {
    name: 'CreateProveedorApp',

    data() {
        return {
            proveedor: {
                rol: ''
            },
        }
    },

    methods: {
        validar() {
            if (!this.proveedor.nombre) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese los nombres',
                    type: 'error'
                });
            } else if (!this.proveedor.email) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el email',
                    type: 'error'
                });
            } else if (!this.proveedor.celular) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el número de celular',
                    type: 'error'
                });
            } else {
                this.crear_proveedor();
            }
        },

        crear_proveedor() {
            axios.post(this.$url + '/registro_proveedor_admin/', this.proveedor, {
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': this.$store.state.token
                }
            }).then((result) => {
                console.log(result);
                if (result.data.data == undefined) {
                    this.$notify({
                        group: 'foo',
                        title: 'ERROR',
                        text: result.data.message,
                        type: 'error'
                    });
                }else{
                    this.$notify({
                        group: 'foo',
                        title: 'SUCCESS',
                        text: 'Se registró correctamente al nuevo proveedor.',
                        type: 'success'
                    });

                    this.$router.push({name: 'proveedor-index'});
                }
            }).catch((error) => {
                console.log(error);
            });
        }
    },

    mounted() {

    },

    components: {
        Sidebar,
        TopNav
    },


}
</script>
