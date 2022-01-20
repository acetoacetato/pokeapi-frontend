<template>
  <div class="container">
    <div class="header">
      <h1>Pokeapi Frontend</h1>
      <PokemonSearch :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
    </div>
    <PokemonList :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />

    <PokemonDetail
      v-if="showDetail"
      :pokemonUrl="pokemonUrl"
      @closeDetail="closeDetail"
    />
  </div>
</template>
<script>
import PokemonSearch from "./PokemonSearch.vue";
import PokemonList from "./PokemonList.vue";
import PokemonDetail from "./PokemonDetail.vue";

export default {
  data: () => {
    return {
      apiUrl: "https://pokeapi-backend.herokuapp.com/pokeapi/",
      pokemonUrl: "",
      showDetail: false,
    };
  },
  components: {
    PokemonSearch,
    PokemonList,
    PokemonDetail,
  },
  methods: {
    setPokemonUrl(url) {
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = "";
      this.showDetail = false;
    },
  },
};
</script>
<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  padding: 10px;
  font-family: "Acme", arial;
}

h1 {
  color: #0127ff;
  padding: 20px;
}

.header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding-bottom: 0px;
}
</style>