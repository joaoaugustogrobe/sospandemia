<template>
  <div class="h-100">
    <form method="post" :action="logoutUrl" ref="logoutForm" style="display: none;" />
    <loading-screen v-if="!uiLoaded"></loading-screen>
    <div class="row h-100 no-gutters" v-else>
      <div class="col">
        <nav class="navbar navbar-expand-lg navbar-light bg-light shadow-sm">
          <div class="container p-0">
            <router-link class="navbar-brand p-0" to="/">
              <img src="../../assets/logo_app.png" height="50" alt />
            </router-link>
            <button
              class="navbar-toggler"
              type="button"
              data-toggle="collapse"
              data-target="#navbarSupportedContent"
              aria-controls="navbarSupportedContent"
              aria-expanded="false"
              aria-label="Toggle navigation"
            >
              <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarSupportedContent">
              <ul class="navbar-nav mr-auto">
                <li class="nav-item" :class="activeClass('gerenciar-demandas')" v-if="activeEntity">
                  <router-link class="nav-link" to="/gerenciar-demandas">Gerenciar Demandas</router-link>
                </li>
                <!-- link para acessar "gerenciar entidade" -->
                <li
                  class="nav-item"
                  :class="activeClass('gerenciar-entidades-local')"
                  v-if="isLogged"
                >
                  <router-link class="nav-link" to="/gerenciar-entidades">Gerenciar Entidades</router-link>
                </li>
                <!--                            <li class="nav-item">-->
                <!--                                <a class="nav-link disabled" href="#">Disabled</a>-->
                <!--                            </li>-->
              </ul>
              <ul class="navbar-nav">
                <li class="nav-item dropdown mr-3" v-if="isLogged">
                  <a
                    class="nav-link dropdown-toggle"
                    href="#"
                    id="entityDropdown"
                    role="button"
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false"
                  >
                    <span class="fa fa-building"></span>&nbsp;
                    <span v-if="activeEntity">{{ activeEntity.name }}</span>
                    <span v-else>Selecione a Entidade</span>
                  </a>
                  <div class="dropdown-menu dropdown-menu-right" aria-labelledby="entityDropdown">
                    <a
                      class="dropdown-item"
                      href="#"
                      v-for="entity in otherEntities"
                      @click="setActiveEntity(entity.id)"
                    >{{ entity.name }}</a>
                    <div class="dropdown-divider" v-if="otherEntities.length > 0"></div>
                    <router-link class="dropdown-item" to="gerenciar-entidades">
                      <span class="fa fa-cogs"></span> Gerenciar Entidades
                    </router-link>
                  </div>
                </li>
                <li class="nav-item dropdown" v-if="isLogged">
                  <a
                    class="nav-link dropdown-toggle"
                    href="#"
                    id="userDropdown"
                    role="button"
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false"
                  >
                    <span class="fa fa-user-check"></span>
                    {{ userName }}
                  </a>
                  <div class="dropdown-menu dropdown-menu-right" aria-labelledby="userDropdown">
                    <!--                                        <a class="dropdown-item" href="#">Action</a>-->
                    <!--                                        <a class="dropdown-item" href="#">Another action</a>-->
                    <!--                                        <div class="dropdown-divider"></div>-->
                    <a class="dropdown-item" href="#" @click="logout()">
                      <span class="fa fa-user-times"></span> Sair
                    </a>
                  </div>
                </li>
                <template v-else>
                  <li class="nav-item">
                    <a class="nav-link" :href="loginUrl">
                      <span class="fa fa-user"></span> Login
                    </a>
                  </li>
                  <li>
                    <a class="nav-link" :href="registerUrl">
                      <span aria-hidden="true" class="fa fa-sign-in-alt"></span> Registrar
                    </a>
                  </li>
                </template>
              </ul>
            </div>
          </div>
        </nav>
        <div class="h-100 d-flex flex-column">
          <div class="row main-content-container main-content-container-colors flex-grow-1">
            <div class="container mt-4">
              <router-view></router-view>
            </div>
          </div>
        </div>

        <div class="container">

          <!-- início mobile -->

          <div class="d-lg-none">
              <div class="row">
         

          <div class="col-4 col-md">
            <h5>Apoio</h5>
            <ul class="list-unstyled text-small">
              <li>
                <a class="text-muted" href="http://www.utfpr.edu.br/" target="_blank">
                  <img class="mb-2" src="../../assets/logo_utfpr.png" height="15" />
                </a>
              </li>
              <li>
                <a class="text-muted" href="http://www.uepg.br/" target="_blank">
                  <img class="mb-2" src="../../assets/logo_uepg.png" height="15" />
                </a>
              </li>
              <li>
                <a
                  class="text-muted"
                  href="https://www.facebook.com/escritoriodecriatividade/"
                  target="_blank"
                >
                  <img class="mb-2" src="../../assets/logo_criatividade.png" height="15" />
                </a>
              </li>
              <li>
                <a
                  class="text-muted"
                  href=" jeferson@webage.solutions" target="_blank"> <img class="mb-2" src="../../assets/logo_webAge.png" height="15" />
                </a>
              </li>
            </ul>
          </div>
          <div class="col-8 col-md">
            <h5>Sobre</h5>
            <ul class="list-unstyled text-small">
              <li>
                <router-link to="sobre-nos">Equipe</router-link>
              </li>
              <li>
                <router-link to="termos-de-uso-e-privacidade">Termo de uso e privacidade</router-link>
              </li>
            </ul>
          </div>
        </div>
<!-- final mobile -->
<div><p class="text-center">contato@sospandemia.org</p></div>
        </div>
<!-- Conteúdo em monitores -->
      <div class="d-none d-lg-block">
        <div class="row">
          <div class="col-12 col-md">
            <img class="mb-2" src="../../assets/logo_app.png" height="50" />
            <small class="d-block mb-3 text-muted">© 2020</small>
          </div>
          <div class="col-6 col-md">
            <h5>Apoio</h5>
            <ul class="list-unstyled text-small">
              <li>
                <a class="text-muted" href="http://www.utfpr.edu.br/" target="_blank">
                  <img class="mb-2" src="../../assets/logo_utfpr.png" height="40" />
                </a>
              </li>
              <li>
                <a class="text-muted" href="http://www.uepg.br/" target="_blank">
                  <img class="mb-2" src="../../assets/logo_uepg.png" height="40" />
                </a>
              </li>
              <li>
                <a
                  class="text-muted"
                  href="https://www.facebook.com/escritoriodecriatividade/"
                  target="_blank"
                >
                  <img class="mb-2" src="../../assets/logo_criatividade.png" height="40" />
                </a>
              </li>
              <li>
                <a
                  class="text-muted"
                  href=" jeferson@webage.solutions" target="_blank"> <img class="mb-2" src="../../assets/logo_webAge.png" height="40" />
                </a>
              </li>
            </ul>
          </div>
          <div class="col-6 col-md">
            <h5>Sobre</h5>
            <ul class="list-unstyled text-small">
              <li>
                <router-link to="sobre-nos">Equipe</router-link>
              </li>
             <li>
                <router-link to="termos-de-uso-e-privacidade">Termo de uso e privacidade</router-link>
              </li>
              <li>contato@sospandemia.org</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
      </div>
    </div>


  </div>
</template>

<script>
import LoadingScreen from "./sections/LoadingScreen";
import api from "../../api";

export default {
  name: "main-layout",
  components: { LoadingScreen },
  data: () => ({}),
  mounted() {},
  methods: {
    logout() {
      api.logout().then(() => {
        // once logged out from the api, logout on the main app
        this.$refs.logoutForm.submit();
      });
    },
    setActiveEntity: function(entityId) {
      this.$store.commit("setEntity", entityId);
    },
    activeClass: function(routeName) {
      return this.$route.name === routeName ? "active" : "";
    }
  },
  computed: {
    isLogged: function() {
      return this.$store.getters.isLogged;
    },
    userName: function() {
      const state = this.$store.state;
      return state.profile ? state.profile.name : null;
    },
    logoutUrl: function() {
      return `${api.baseURL()}/auth/logout`;
    },
    activeEntity: function() {
      const store = this.$store;
      return store.getters.activeEntity;
    },
    otherEntities: function() {
      const store = this.$store;
      const entities = store.getters.entities;
      return entities
        ? entities.filter(entity => entity.id !== store.getters.activeEntityId)
        : [];
    },
    loginUrl: function() {
      return api.baseURL() + "/auth/login";
    },
    registerUrl: function() {
      return api.baseURL() + "/auth/register";
    },
    uiLoaded: function() {
      return this.$store.state.uiLoaded;
    }
  }
};
</script>

<style>
.modal.left .modal-dialog {
  position: fixed;
  left: 0;
  margin: auto;
  width: 280px;
  height: 100%;
  -webkit-transform: translate3d(0%, 0, 0);
  -ms-transform: translate3d(0%, 0, 0);
  -o-transform: translate3d(0%, 0, 0);
  transform: translate3d(0%, 0, 0);
}

.modal.left .modal-content {
  height: 100%;
  overflow-y: auto;
}

/* ----- MODAL STYLE ----- */
.modal-content {
  border-radius: 0;
  border: none;
}

.main-content-container {
}

.top-logo {
  height: 22px;
}

.avatar {
  display: inline-block;
  position: relative;
  background: grey;
  width: 256px;
  height: 256px;
  line-height: 256px;
  color: #fff;
  text-align: center;
  border-radius: 300px;
  background-size: cover !important;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  -webkit-box-flex: 0;
  -ms-flex-positive: 0;
  flex-grow: 0;
  -ms-flex-negative: 0;
  flex-shrink: 0;
  -webkit-box-sizing: content-box;
  box-sizing: content-box;
}

.avatar-24 {
  width: 24px;
  height: 24px;
  line-height: 24px;
  font-size: 10px;
}

.avatar-32 {
  width: 32px;
  height: 32px;
  line-height: 32px;
  font-size: 18px;
}

.avatar-48 {
  width: 48px;
  height: 48px;
  line-height: 48px;
  font-size: 24px;
}

.avatar-64 {
  width: 64px;
  height: 64px;
  line-height: 64px;
  font-size: 32px;
}

.avatar-96 {
  width: 96px;
  height: 96px;
  line-height: 96px;
  font-size: 40px;
}

.avatar-128 {
  width: 128px;
  height: 128px;
  line-height: 128px;
  font-size: 50px;
}
.pt-md-5,
.py-md-5 {
  padding-top: 3rem !important;
}
.mb-md-5,
.my-md-5 {
  margin-bottom: 3rem !important;
}
.mt-md-5,
.my-md-5 {
  margin-top: 3rem !important;
}
.pt-4,
.py-4 {
  padding-top: 1.5rem !important;
}
.border-top {
  border-top: 1px solid #dee2e6 !important;
}
article,
aside,
figcaption,
figure,
footer,
header,
hgroup,
main,
nav,
section {
  display: block;
}
*,
::after,
::before {
  box-sizing: border-box;
}
</style>