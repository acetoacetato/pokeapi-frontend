<template>
  <div class="list">
    <article
      v-for="(pokemon, name) in pokemons"
      :key="'poke' + name"
      @click="setPokemonUrl(pokemon.id)"
    >
      <img :src="pokemon.sprite" width="96" height="96" alt="" />

      <h3>{{ pokemon.name }}</h3>
      Peso: {{ pokemon.weight }} <br />
      Tipo: {{ pokemon.type }} <br />
      Habilidades:
      <ul>
        <li v-for="ability in pokemon.abilities" :key="'abi' + ability.name">
          <b>{{ ability.name }}</b>
        </li>
      </ul>
    </article>
    <div id="scroll-trigger" ref="infinitescrolltrigger">
      <i class="fas fa-spinner fa-spin"></i>
    </div>
  </div>
</template>

<script>
export default {
  props: ["apiUrl"],
  data: () => {
    return {
      pokemons: [],
      nextUrl: "",
      currentUrl: "",
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.currentUrl);
      console.log(req);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          this.nextUrl = this.apiUrl + data.nextUrl;
          console.log(this.nextUrl);
          console.log(data);
          data.results.forEach((pokemon) => {
            pokemon.url = this.apiUrl + pokemon.id;
            pokemon.type = pokemon.type.join(" / ");
            console.log(pokemon);
            this.pokemons.push(pokemon);
          });
        })
        .catch((error) => {
          console.log(error);
        });
    },
    scrollTrigger() {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach((entry) => {
          if (entry.intersectionRatio > 0 && this.nextUrl) {
            this.next();
          }
        });
      });

      observer.observe(this.$refs.infinitescrolltrigger);
    },
    next() {
      this.currentUrl = this.nextUrl;
      console.log(this.nextUrl);
      this.fetchData();
    },
    setPokemonUrl(id) {
      this.$emit("setPokemonUrl", this.apiUrl + id);
    },
  },
  created() {
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  mounted() {
    this.scrollTrigger();
  },
};
</script>

<style lang="scss" scoped>
.list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  grid-gap: 10px;
  width: 100%;
  max-width: 90%;
}
article {
  height: 300px;
  background-color: #efefef;
  text-align: center;
  text-transform: capitalize;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
h3 {
  margin: 0;
}
#scroll-trigger {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 150px;
  font-size: 2rem;
  color: #efefef;
}
</style>

