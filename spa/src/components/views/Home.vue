<template>
  <div class="home">
    <v-container fluid>
      <v-row>
        <v-col cols="12" md="3" order="2" order-md="1">
          <v-col cols="12">
            <relatorio-numerico title="Pedidos" :qtd="100"></relatorio-numerico>
          </v-col>
          <v-col cols="12">
            <relatorio-numerico title="Atendidos" :qtd="54"></relatorio-numerico>
          </v-col>
        </v-col>
        <v-col cols="12" md="6" order="1" order-md="2">
          <div class="row justify-content-center">
            <v-img :src="require('../../assets/logo_app.png')" max-width="300px"></v-img>
          </div>
        </v-col>
        <v-col cols="12" md="3" order="3" order-md="3">
          <v-col cols="12">
            <relatorio-numerico title="Cidades" :qtd="10"></relatorio-numerico>
          </v-col>
          <v-col cols="12">
            <relatorio-numerico title="Usuários" :qtd="23"></relatorio-numerico>
          </v-col>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div class="container-fluid">
            <div class="row justify-content-center">
              <p>É uma iniciativa solidária conjunta UTFPR e UEPG que têm como objetivo uma infraestrutura de comunicação que visa integrar as unidades dos sistemas de saúde e projetos sociais conectando a quem pode contribuir com as demandas.</p>
            </div>
            <v-spacer></v-spacer>
            <v-btn @click="help=true;">
              Primeiros Passos
            </v-btn>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <!-- <p>
      Primeira vez utilizando o sistema? <a @click="help=true">Clique aqui!</a>
    </p> -->
    <v-dialog v-model="help" max-width="600px">
      <v-card class="">
        <v-card-title class="headline">
          Bem vindo ao SOS Pandemia!
        </v-card-title>
        <v-card-text>
          Gostaria de cadastrar minha entidade para receber ajuda,  <router-link to="primeiros-passos">como proceder?</router-link> <a></a> <!-- adicionar link do video aqui @Hernani -->
        </v-card-text>
        <v-divider></v-divider>
        <v-card-text>
          Gostaria de ajudar entidades necessitadas, <a @click="donation = true">como proceder?</a>
          <v-card-text v-if="donation">
            <p>
              Na página inicial, são encontradas as necessidades entidades que necessitam de doações, nela é possível buscar por uma necessidade específica,
              filtrar as necessidades por estado e cidade, além de poder procurar por uma entidade específica.
            </p>
            <p>Para visualizar mais informações sobre a empresa necessitada, basta clicar no nome dela que se encontra na parte inferior da demanda.</p>
            <p>E por fim, basta entrar em contato com a entidade necessitada, por meio de um dos contatos disponibilizados pela mesma
              (se encontra na demanda ou nas informações detalhadas da empresa).</p>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              v-if="donation"
              color="primary"
              text
              @click="$vuetify.goTo('#start');help=false;"
            >entendi, direcionar para demandas</v-btn>
          </v-card-actions>
        </v-card-text>
        <v-divider></v-divider>
        <v-card-text>
          email: contato@sospandemia.org
        </v-card-text>
        <v-divider></v-divider>
        <v-card-text>
          <ul class="list-inline text-small">
            <li>Apoio</li>
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
              <li><router-link to="sobre-nos">Equipe</router-link></li>
            </ul>

        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="red darken-1"
              text
              @click="help = false"
            >
              Fechar
            </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-dialog v-model="entityDetails" max-width="600px">
      <v-card>
        <v-card-title class="headline grey lighten-2" primary-title>
          Detalhes da entidade
        </v-card-title>
        <div class="card-body">

        <v-card-text>
          <p><strong>Nome: </strong> {{currentEntity.name}}</p>
          <p><strong>Razão Social: </strong> {{currentEntity.legal_name}}</p>
          <p><strong>CNPJ: </strong> {{currentEntity.cnpj}}</p>
          <p v-if="currentEntity.entity_type_id==1">
            <strong>CNES: </strong> {{currentEntity.entity_type_document}}
          </p>
          <p><strong>Endereço: </strong> {{currentEntity.street_address}}</p>
          <p><strong>Cidade - Estado: </strong> {{currentEntity.city}}</p>
          <p><strong>Contato: </strong> {{currentEntity.contact_info}}</p>
          <p><strong>Sobre: </strong> {{currentEntity.description}}</p>
        </v-card-text>
        </div>
      <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
              color="red darken-1"
              text
              @click="entityDetails = false"
            >
              Fechar
            </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <div id="start" class="form-group">
      <v-text-field
        type="email"
        class="w-50 m-auto"
        aria-describedby="emailHelp"
        placeholder="Busca..."
        outlined
        v-model="query"
      />
      <small id="emailHelp" class="form-text text-muted">Começe a digitar para filtar os resultados.</small>
    </div>
    <v-btn text @click="handleAdvancedFilterState">
      <span>Filtro avançado</span>
    </v-btn>
    <div v-if="showFilter === true">
      <v-row>
        <v-col cols="2">
          <v-select
            label="Estado"
            :items="states"
            v-model="filterOptions.state"
            :loading="!statesFetched"
            @change="search(query)"
            outlined
            item-text="uf"
            item-value="id"
          ></v-select>
        </v-col>
        <v-col cols="10">
          <DistrictSelector :stateId="filterOptions.state" :disabled="filterOptions.state == 0"  :onChangeCB="onCityChange"/>
        </v-col>
      </v-row>
      <small id="emailHelp" class="form-text text-muted">
        Buscando resultados em
        <span v-if="filterOptions.state === 0">Brasil.</span>
        <span
          v-else-if="this.filterOptions.city != null &&
          typeof this.filterOptions.city == 'object'"
        >{{this.filterOptions.city.name}}.</span>
        <span v-else>{{stateName}}</span>
      </small>
    </div>
    <hr />
    <loading-widget v-if="widgetLoading==true"></loading-widget>
    <p
      v-else-if="widgetLoading==false && demands.length === 0"
    >Não há demandas correspondente à busca!</p>
    <div class="results" v-else>
      <div class="card m-3" v-for="demand in demands" v-bind:key="demand.id">
        <div class="card-header">
          <h4>{{ demand.title}}</h4>
        </div>
        <div class="card-body">
          {{ demand.text}}
          <hr v-if="demand.quantity" />
          <p v-if="demand.quantity">Quantidade: {{ demand.quantity }} {{ demand.unit }}</p>
          <hr v-if="demand.contact_info || demand.entity.contact_info" />
          <h5 v-if="demand.contact_info || demand.entity.contact_info">Informações de Contato:</h5>
          <hr v-if="demand.contact_info || demand.entity.contact_info" style="width: 300px"/>
          <p v-if="demand.entity.contact_info"><strong>Entidade:</strong> {{ demand.entity.contact_info }}</p>
          <p v-if="demand.contact_info"><strong>Específico da Demanda:</strong> {{ demand.contact_info }}</p>

        </div>
        <div
          class="card-footer"
        ><a @click="showEntityDetails(demand.entity)">{{ demand.entity.name }}</a> - {{ demand.entity.city }}</div>
      </div>
    </div>
    <div class="text-center mt-2" v-if="widgetLoading==false && last_page>1">
    <v-pagination
      v-model="current_page"
      @input="search"
      :length="last_page"
    ></v-pagination>
  </div>
  </div>
</template>

<script>
import api from "../../api";
import _ from "lodash";
import LoadingWidget from "../widgets/LoadingWidget";
import DistrictSelector from "../widgets/DistrictSelector";
import RelatorioNumerico from "@/components/card-relatorio/RelatorioNumerico";

export default {
  name: "Home",
  components: { LoadingWidget, DistrictSelector, RelatorioNumerico },
  data() {
    return {
      entityDetails: false,
      currentEntity: {},
      help:false,
      donation:false,
      current_page:1,
      last_page:1,
      query: "",
      widgetLoading: false,
      demands: [],
      showFilter: false,
      statesFetched: false,
      states: [],
      filterOptions: {
        state: 0,
        city: null,
      }
    };
  },
  computed: {
    stateName: function() {
      return this.states.find(el => {
        return this.filterOptions.state == el.id
      }).name
    }
  },
  methods: {
    showEntityDetails: function(entity){
      this.entityDetails = true;
      this.currentEntity = entity;
    },
    search: function(query) {
      this.widgetLoading = true;
      if (this.showFilter && this.filterOptions.state != 0) {
        let filterType;
        let filterParam;
        if (
          this.filterOptions.city != null &&
          typeof this.filterOptions.city == "object"
        ) {
          filterType = "district_id";
          filterParam = this.filterOptions.city.id;
        } else {
          filterType = "state_id";
          filterParam = this.filterOptions.state;
        }
        api
          .searchDemands(this.query, 1, filterType, filterParam)
          .then(({ data }) => {
            this.demands = data.data;
            this.last_page = data.last_page;
            this.widgetLoading = false;
          });
      } else {
        api.searchDemands(this.query,this.current_page).then(({ data }) => {
          this.last_page = data.last_page;
          this.demands = data.data;
          this.widgetLoading = false;
        });
      }
    },
    handleAdvancedFilterState() {
      this.showFilter = !this.showFilter;
      if (this.showFilter === false) return;

      if (this.statesFetched === false) this.fetchStates();
    },
    fetchStates() {
      api.getStates().then(res => {
        this.states = res.data;
        this.states.sort((a, b) => {
          const stateA = a.uf;
          const stateB = b.uf;
          let comparison = 0;
          if (stateA > stateB) {
            comparison = 1;
          } else if (stateA < stateB) {
            comparison = -1;
          }
          return comparison;
        });
        this.statesFetched = true;
      });
    },
    onCityChange(city){
      this.filterOptions.city = city
    }
  },
  watch: {
    help: function(val){
      if(val == false){
        this.donation = false;
      }
    },
    query: _.debounce(function() {
      this.current_page = 1;
      this.search();
    }, 300),
  },
  mounted: function() {
    this.search();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.home {
  text-align: center;
}
.results {
  text-align: left;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
