<template>
  <v-autocomplete
    :ref="refBind"
    v-model="city"
    :disabled="disabled"
    :rules="rules"
    :items="cities"
    item-text="name"
    label="Cidade"
    autocomplete="null"
    placeholder="Digite o nome da cidade para buscar"
    :search-input.sync="search"
    outlined
    :no-data-text="noDataText"
    hide-selected
    return-object
  ></v-autocomplete>
</template>

<script>
import api from "../../api";
export default {
  props: {
    stateId: {
      type: Number,
      required: true
    },
    onChangeCB: {
      type: Function,
      required: false,
      default: () => {}
    },
    rules: {
      type: Array,
      required: false
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false
    },
    defaultValue: {
      type: Object,
      required: false
    },
    refBind: {
      type: String,
      required: false,
      default: "autocomplete-city"
    }
  },
  data() {
    return {
      search: null,
      debounce: null,
      noDataText: "Continue digitando para encontrar uma cidade.",
      cities: [],
      city: null,
      validState: true
    };
  },
  methods: {
    fetchCities(stateId, query) {
      return api.getDistricts(stateId, query);
    }
  },
  watch: {
    search(query) {
      if (query === null) return;
      if (query.length < 3) {
        this.noDataText = "Continue digitando para encontrar uma cidade.";
        return;
      }
      clearTimeout(this.debounce);
      let that = this;
      this.debounce = setTimeout(function() {
        that
          .fetchCities(that.stateId, query)
          .then(res => {
            that.cities = res.data;
            that.noDataText =
              "Nenhuma cidade encontrada. Verifique a busca ou seja mais específico";
          })
          .catch(res => {
            if (res.response.status == "404")
              that.noDataText = "Estado inválido.";
          });
      }, 300);
    },
    city() {
      if (this.onChangeCB) this.onChangeCB(this.city);
    }
  },
  created() {
    if (this.defaultValue) {
      this.cities.push(this.defaultValue);
      this.city = this.cities[0];
    }
  }
};
</script>

<style>
</style>