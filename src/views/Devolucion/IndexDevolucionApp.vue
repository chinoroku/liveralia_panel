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
                        Devoluciones
                      </h6>
  
                      <!-- Title -->
                      <h1 class="header-title">
                        Nueva Devolucion
                      </h1>
  
                    </div>
                  </div> <!-- / .row -->
                  <div class="row align-items-center">
                    <div class="col">
  
                      <!-- Nav -->
                      <ul class="nav nav-tabs nav-overflow header-tabs">
                        <li class="nav-item">
                          <a class="nav-link active">
                            Todos las Devoluciones
                          </a>
                        </li>
                        <li class="nav-item">
                          <router-link to="/devolucion/create" class="nav-link">Nueva Devolucion</router-link>
                        </li>
  
                      </ul>
  
                    </div>
                  </div>
                </div>
              </div>
  
              <div class="tab-content">
                <div class="tab-pane fade show active" id="contactsListPane" role="tabpanel"
                  aria-labelledby="contactsListTab">
  
                  <!-- Card -->
                  <div class="card"
                    data-list='{"valueNames": ["item-name", "item-title", "item-email", "item-phone", "item-score", "item-company"], "page": 10, "pagination": {"paginationClass": "list-pagination"}}'
                    id="contactsList">
                    <div class="card-header">
                      <div class="row align-items-center">
                        <div class="col">
  
                          <!-- Form -->
                          <form>
                            <div class="input-group input-group-flush input-group-merge input-group-reverse">
                              <input class="form-control list-search" type="search" v-model="filtro"
                                placeholder="Buscar Devolucion">
                              <span class="input-group-text" style="cursor:pointer" v-on:click="init_data()">
                                <i class="fe fe-search"></i>
                              </span>
                            </div>
                          </form>
  
                        </div>
                      </div> <!-- / .row -->
                    </div>
                    <div class="table-responsive">
                      <table class="table table-sm table-hover table-nowrap card-table">
                        <thead>
                          <tr>
  
                            <th>
                              <a class="list-sort text-muted">Serie Devolucion</a>
                            </th>
                            <th>
                              <a class="list-sort text-muted">Nombre Cliente</a>
                            </th>
                            <th>
                              <a class="list-sort text-muted">Monto</a>
                            </th>
                            <th>
                              <a class="list-sort text-muted">Estado</a>
                            </th>
                          </tr>
                        </thead>
                        <paginate tag="tbody" ref="proveedores" name="proveedores" :list="proveedores" :per="perPage"
                          class="list fs-base">
                          <tr v-if="!load_data" v-for="item in paginated('proveedores')">
  
                            <td>
  
                             <a class="item-name text-reset">{{ item.serie }}</a>
  
                            </td>
                            <td>
  
                              <!-- Text -->
                              <span class="item-title">{{ item.cliente.nombres }}</span>
  
                            </td>
                            <td>
  
                              <!-- Email -->
                              <a class="item-email text-reset">{{ item.total }}</a>
  
                            </td>
  
                            <td>
  
                              <!-- Badge -->
                              <span v-if="!item.estado" class="item-score badge bg-danger-soft">Desactivado</span>
                              <span v-if="item.estado" class="item-score badge bg-success-soft">Activo</span>
  
                            </td>

                          </tr>
                        </paginate>
                        <tr v-if="load_data">
                          <td colspan="5" class="text-center">
                            <div class="spinner-border mt-5 mb-5" role="status">
                              <span class="visually-hidden">Loading...</span>
                            </div>
                          </td>
                        </tr>
                      </table>
                    </div>
                    <div class="card-footer d-flex justify-content-between">
  
                      <!-- Pagination (prev) -->
                      <ul class="list-pagination-prev pagination pagination-tabs card-pagination">
                        <li class="page-item">
                          <a class="page-link ps-0 pe-4 border-end" v-on:click="goPrev()">
                            <i class="fe fe-arrow-left me-1"></i> Anterior
                          </a>
                        </li>
                      </ul>
  
                      <!-- Pagination -->
                      <paginate-links @change="onLangsPageChange" for="colaboradores" :classes="{
                        'ul': ['list-pagination', 'pagination', 'pagination-tabs',
                          'card-pagination'], 'a': ['page']
                      }"></paginate-links>
  
                      <!-- Pagination (next) -->
                      <ul class="list-pagination-next pagination pagination-tabs card-pagination">
                        <li class="page-item">
                          <a class="page-link ps-4 pe-0 border-start" v-on:click="goNext()">
                            Siguiente <i class="fe fe-arrow-right ms-1"></i>
                          </a>
                        </li>
                      </ul>
  
                    </div>
                  </div>
  
                </div>
  
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
  import store from '@/store/index';
  
  export default {
    name: 'IndexDevolucionApp',
    data() {
      return {
        proveedores: [],
        proveedores_const: [],
        paginate: ['proveedores'],
        currentPage: 1,
        perPage: 15,
        filtro: '',
        load_data: false,
        valorDeLaVariable:''
      }
    },
    components: {
      Sidebar,
      TopNav,
    },
    methods: {
      onLangsPageChange(toPage, fromPage) {
        // handle here…
        this.currentPage = toPage;
        console.log(this.currentPage);
      },
      goPrev() {
        if (this.currentPage >= 2) {
          this.$refs.proveedores.goToPage(this.currentPage--);
        } else {
          this.$refs.proveedores.goToPage(1);
        }
      },
      goNext() {
        if (this.currentPage <= Math.ceil(this.proveedores.length / this.perPage)) {
          this.$refs.proveedores.goToPage(this.currentPage++);
        } else {
          this.$refs.proveedores.goToPage(Math.ceil(this.proveedores.length / this.perPage));
        }
      },
      filtrar() {
        console.log(this.filtro);
        let term = new RegExp(this.filtro, 'i');
        //this.colaboradores = this.colaboradores_const.filter(item=>term.test(item.nombres)||term.test(item.apellidos)||term.test(item.email));
        this.init_data();
      },
      init_data() {
        this.load_data = true;
        axios.get(this.$url + '/listar_devolucion_admin/' + this.filtro, {
          headers: {
            'Content-Type': 'application/json',
            'Authorization': store.state.token,
          }
        }).then((result) => {
          this.proveedores = result.data;
          this.proveedores_const = this.proveedores;
          this.load_data = false;
          console.log(this.proveedores);
        }).catch((error) => {
          console.log(error);
        });
      },
      eliminar(id, estado) {
        axios.put(this.$url + '/cambiar_estado_proveedor_admin/' + id, { estado: estado }, {
          headers: {
            'Content-Type': 'application/json',
            'Authorization': this.$token
          }
        }).then((result) => {
          this.init_data();
          this.$notify({
            group: 'foo',
            title: 'SUCCESS',
            text: 'Se cambió el estado del proveedor.',
            type: 'success'
          });
        });
      }
    },
    beforeMount() {
      this.init_data();
    },
  }
  </script>
    