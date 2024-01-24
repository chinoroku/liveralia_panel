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
                      <h6 class="header-pretitle">Devoluciones</h6>
                      <!-- Title -->
                      <h1 class="header-title">Nuevo Devolucion</h1>
                    </div>
                  </div>
                  <div class="row align-items-center">
                    <div class="col">
                      <!-- Nav -->
                      <ul class="nav nav-tabs nav-overflow header-tabs">
                        <li class="nav-item">
                          <router-link to="/devolucion" class="nav-link">Todos las Devoluciones</router-link>
                        </li>
                        <li class="nav-item">
                          <a class="nav-link active">Nuevo Devolucion</a>
                        </li>
                      </ul>
                    </div>
                  </div>
                </div>
              </div>
  
              <div class="mb-7">
  
                <!-- Divider -->
                <hr class="my-5">
  
                <div class="row">
                  <div class="col-12 col-md-12">
                    <!-- Title -->
                    <div class="form-group">
                      <!-- Label -->
                      <label class="mb-1">
                                            Título del Devolucion
                                        </label>

                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            Elegir un título adecuado para el Devolucion.
                                        </small>

                                        <!-- Input -->
                                        <input type="text" class="form-control" placeholder="Título del Devolucion"
                                            v-model="devolucion.titule">
                    </div>
                  </div>
  
                  <div class="col-12 col-md-6">
                    <!-- Clientes -->
                    <div class="form-group">
                      <!-- Label -->
                      <label class="form-label">
                                            Clientes
                                        </label>
                                        
                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            Seleccionar un cliente para la devolucion.
                                        </small>

                                        <!-- Input -->
                                        <select name="" class="form-select" v-on:change="getVentas($event)">
                                            <option value="" disabled selected>Seleccionar</option>
                                            <option :value="item._id" v-for="item in clientes">{{item.nombres}}</option>
                                        </select>
                    </div>
                  </div>
  
                  <div class="col-12 col-md-6">
                    <!-- Ventas -->
                    <div class="form-group">
                      <!-- ... (Ventas select input) ... -->
                                        <!-- Label -->
                                        <label class="form-label">
                                            Ventas
                                        </label>

                                        <!-- Form text -->
                                        <small class="form-text text-muted">
                                            Seleccionar una venta para la devolucion.
                                        </small>

                                    <!-- Input -->
                                    <select name="" class="form-select" v-on:change="getdetalleVentas($event)">
                                        <option value="" disabled selected>Seleccionar</option>
                                        <option :value="item._id" v-for="item in ventas">{{item.transaccion}}</option>
                                    </select>

                    </div>
                  </div>
                </div>
  
                <div class="table-responsive">
                  <!-- Table -->
                  <table class="table table-sm table-hover table-nowrap card-table">
                    <thead>
                      <!-- Table headers -->
                      <tr>
                        <th><a class="list-sort text-muted">Nombre</a></th>
                        <th><a class="list-sort text-muted">Cantidad</a></th>
                        <th><a class="list-sort text-muted">Precio</a></th>
                        <th><a class="list-sort text-muted">Acciones</a></th>
                      </tr>
                    </thead>
                    <!-- Paginated table body -->
                    <paginate tag="tbody" ref="detalleventas" name="detalleventas" :list="detalleventas" :per="perPage"
                      class="list fs-base">
                      <tr v-if="!load_data" v-for="item in paginated('detalleventas')">
                        <!-- Table row content -->
                        <td>
                          <!-- Avatar -->
                          <a href="#!" class="avatar avatar-lg">
                            <img :src="$url + '/obtener_portada_producto/' + item.producto.portada"
                              alt="..." class="avatar-img rounded">
                          </a>
                          <a class="item-name text-reset">{{ item.producto.titulo }}</a>
                        </td>
                        <td>
                          <!-- Text -->
                          <span class="item-title">{{ item.cantidad }}</span>
                        </td>
                        <td>
                          <!-- Email -->
                          <a class="item-email text-reset">{{ convertCurrency(item.precio_unidad) }}</a>
                        </td>
                        <td class="text-end">
                          <!-- Input and Button for adding -->
                          <b-input type="number" v-model="cantidadAgregada" :max="item.cantidad" class="mb-2"></b-input>
                          <b-button variant="primary" v-on:click="agregar(item._id)">
                            <b-icon-plus></b-icon-plus>
                          </b-button>
                        </td>
                      </tr>
                    </paginate>
                    <!-- Loading spinner -->
                    <tr v-if="load_data">
                      <td colspan="5" class="text-center">
                        <div class="spinner-border mt-5 mb-5" role="status">
                          <span class="visually-hidden">Loading...</span>
                        </div>
                      </td>
                    </tr>
                  </table>
                </div>
            <div class="col-3">
                <div class="form-group">
                <label class="mb-1">Monto Nota de Credito</label>
                <input type="text" class="form-control" v-model="valorDesdeVariable" />
                </div>
            </div>
                <!-- Divider -->
                <hr class="mt-3 mb-5">
  
                <!-- Button for creating Devolucion -->
                <button class="btn btn-primary" v-on:click="validad()">Crear Devolucion</button>
  
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
import moment from 'moment';
import currency_formatter from 'currency-formatter';

export default {
    name: 'DevolucionApp',
    components: {
        Sidebar,
        TopNav
    },
    data() {
        return {
            str_image: '/assets/img/avatar-1.jpg',
            devolucion: {
            },
            clientes: [],
            ventas: [],
            detalleventas: [],
            portada: undefined,
            load_data: false,
            id_cliente:'',
            paginate: ['detalleventas'],
            valorDesdeVariable:0,
            cantidadAgregada: 1,
            currentPage: 1,
            perPage: 15,
            get itemsForList() {
                return this.detalleventas.slice(
                    (this.currentPage - 1) * this.perPage, this.currentPage * this.perPage
                )
            }
        }
    },
    beforeMount(){
        this.init_clientes();
    },
    methods: {
        validad() {
            if (!this.devolucion.titule) {
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'Ingrese el título de la devolucion.',
                    type: 'error'
                });
            }else if(this.valorDesdeVariable===0){
                this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'No ha ingresado productos para la devolucion.',
                    type: 'error'
                }); 
            }
             else {
                this.registro();
            }
        },

        registro() {

            for(var item of this.clientes){
                if(item._id == this.id_cliente){
                    this.devolucion.cliente = item._id;
                    this.devolucion.nombre = item.nombres;
                    this.devolucion.email = item.email;
                }
            }

            this.devolucion.titulo = this.devolucion.titule;
            this.devolucion.Monto = this.valorDesdeVariable;
            this.devolucion.detalleventas = this.detalleventas;

            axios.post(this.$url + '/registro_devolucion_admin', this.devolucion, {
                headers: {
                    'Content-Type': 'application/json',
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
                        text: 'Se registró correctamente el devolucion.',
                        type: 'success'
                    });

                    this.$router.push({ name: 'devolucion-index' });
                }
            })

        },
        init_clientes(){
          axios.get(this.$url+'/listar_clientes_devolucion',{
              headers:{
                  'Content-Type': 'application/json',
                  'Authorization': this.$store.state.token,
              }
          }).then((result)=>{
              this.clientes = result.data;
          });
      },
      getVentas(event){
        this.valorDesdeVariable=0;
        
            for(var item of this.clientes){
                if(item._id == event.target.value){
                    this.id_cliente =item._id;
                    axios.get(this.$url+'/listar_ventas_clientes/'+ item._id,{
                        headers:{
                            'Content-Type': 'application/json',
                            'Authorization': this.$store.state.token,
                        }
                    }).then((result)=>{
                        this.ventas = result.data.ventas;
                    });
                }
            }
      },
      getdetalleVentas(event){
        this.valorDesdeVariable=0;
            for(var item of this.ventas){
                if(item._id == event.target.value){
                    this.load_data = true;
                    axios.get(this.$url+'/listar_detalle_ventas/'+ item._id,{
                        headers:{
                            'Content-Type': 'application/json',
                            'Authorization': this.$store.state.token,
                        }
                    }).then((result)=>{
                        this.detalleventas = result.data.venta_detalle;
                        this.load_data = false;

                    });
                }
            }
      },
      agregar(id) {
      // Lógica para manejar el clic del botón
      for (let i = 0; i < this.detalleventas.length; i++) {
        if (this.detalleventas[i]._id === id){
            if(this.cantidadAgregada <= this.detalleventas[i].cantidad) {
            this.detalleventas[i].cantidad -= this.cantidadAgregada;

            this.valorDesdeVariable += this.detalleventas[i].precio_unidad*this.cantidadAgregada;
        }else{
            this.$notify({
                    group: 'foo',
                    title: 'ERROR',
                    text: 'La cantidad ingresada es mayor que la cantidad disponible.',
                    type: 'error'
                });
        }   
        } 
        
        
      }


    },
      convertDate(date) {
            return moment(date).format('YYYY-MM-DD');
        },
        convertCurrency(number) {
            
            return currency_formatter.format(number, { code: 'PEN' });
        }
    },
        
}
</script>
    