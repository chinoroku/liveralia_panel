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
                                            Provedores
                                        </h6>

                                        <!-- Title -->
                                        <h1 class="header-title">
                                            Editar proveedor
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
                                                <router-link to="/proveedor/create" class="nav-link active">Actualizar
                                                    proveedor</router-link>
                                            </li>
                                        </ul>

                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Form -->

                        <template v-if="data">
                            <div>
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
                                                placeholder="Nombres completos">

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
                                    <div class="col-12">

                                        <!-- Email address -->
                                        <div class="form-group">

                                            <!-- Label -->
                                            <label class="mb-1">
                                                Celular
                                            </label>

                                            <!-- Input -->
                                            <input type="celular" class="form-control" v-model="proveedor.celular"
                                                placeholder="Correo electrónico">

                                        </div>

                                    </div>

                                </div> <!-- / .row -->
                                <hr class="my-5">
                                <button type="button" class="btn btn-primary" v-on:click="validar()">
                                    Guardar cambios
                                </button>
                            </div>
                        </template>

                        <template v-if="!data">
                            <div>
                                <ErrorNotFound />
                            </div>
                        </template>

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
import ErrorNotFound from '@/components/ErrorNotFound.vue';
import axios from 'axios';

export default {
    name: 'EditarProveedorApp',

    data() {
        return {
            proveedor: {
                rol: ''
            },
            id: this.$route.params.id,
            data: false,
        }
    },

    methods: {
        init_data() {
            axios.get(this.$url + '/obtener_proveedor_admin/' + this.id, {
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': this.$store.state.token,
                }
            }).then((result) => {
                if (result.data == "") {
                    this.data = false;
                } else {
                    this.data = true;
                    this.proveedor = result.data;
                }
                console.log(this.data);
            }).catch((error) => {
                console.log(error);
            });
        },

        validar() {
            if (!this.proveedor.nombre) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el nombre del proveedor',
                    type: 'error'
                });
            } else if (!this.proveedor.email) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese un correo correcto',
                    type: 'error'
                });
            } else if (!this.proveedor.celular) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese un celular validp',
                    type: 'error'
                });
            }else {
                this.actualizar_proveedor();
            }
        },

        actualizar_proveedor() {
            axios.put(this.$url + '/actualizar_proveedor_admin/' + this.id, this.proveedor, {
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': this.$store.state.token
                }
            }).then((result) => {
                this.$notify({
                    group: 'foo',
                    title: 'SUCCESS',
                    text: 'Se actualizó correctamente al proveedor.',
                    type: 'success'
                });

                this.$router.push({ name: 'proveedor-index' });
            }).catch((error) => {
                console.log(error);
            });
        }
    },

    mounted() {
        console.log(this.id);
    },

    beforeMount() {
        this.init_data();
    },

    components: {
        Sidebar,
        TopNav,
        ErrorNotFound,
    },


}
</script>
