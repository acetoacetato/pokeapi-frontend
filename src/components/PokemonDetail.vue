<template>
  <div class="detail">
    <div class="detail-view card" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="pokemon.sprite" alt="" />
      </div>
      <div v-if="pokemon" class="data card-body">
        <h2 class="card-title">{{ pokemon.name }}</h2>
        <p class="card-text arrow">
          <span> {{ pokemon.description }} </span>
        </p>

        <div class="property">
          <div class="left destaq">Type</div>
          <div class="right">{{ pokemon.type }}</div>
        </div>
        <div class="property">
          <div class="left destaq">Habilities</div>
          <div class="right">
            <ul>
              <li
                v-for="ability in pokemon.abilities"
                :key="'abi' + ability.name"
              >
                <b>{{ ability.name }}</b>
              </li>
            </ul>
          </div>
        </div>

        <div class="property">
          <div class="left destaq">Weight/Peso</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>

        <div class="property">
          <div class="left destaq">Evolutions</div>
          <div class="right">
            {{ pokemon.evolutions }}
          </div>
        </div>
      </div>
      <h2 v-else>El Pokemon no existe :c</h2>
      <button class="close" @click="closeDetail">X</button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>

<script>
export default {
  props: ["pokemonUrl"],
  data: () => {
    return {
      show: false,
      pokemon: {},
    };
  },
  methods: {
    fetchData() {
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200) return resp.json();
        })
        .then((data) => {
          console.log(data);
          // Formats the types of the pokemon with / between them in case there is more than one
          data.type = data.type.join(" / ");
          // Formats the evolution chain with -> between each evolution name
          data.evolutions = data.evolutions.join(" \u2192 ");
          this.pokemon = data;
          this.show = true;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    closeDetail() {
      this.$emit("closeDetail");
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<style lang="scss" scoped>
.arrow {
  text-align: center;
}
i.fa-spinner {
  text-align: center;
}
.detail {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  width: 100%;
  height: 100vh;
  background: rgba(10, 7, 0, 0.562);
}
.detail-view {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 90%;
  padding: 20px 0 0;
  position: relative;
  margin-bottom: 5%;
  max-width: 510px;

  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
.image {
  display: flex;
  justify-content: center;
  align-items: center;
  //position: absolute;
  left: 40px;
  top: 10px;
  border-radius: 20%;
  width: 100px;
  height: 100px;
  background-color: #04d9ff;
  overflow: hidden;
}

h2 {
  text-transform: capitalize;
}

.data {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-bottom: 40px;
}
.property {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
  margin-bottom: 10px;
}
.left {
  float: left;
}
.right {
  float: right;
}
h3 {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
}

.close {
  outline: none;
  border: none;
  border-radius: 100%;
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #c73015;
  color: #efefef;
  padding: 10px 12px;
  //margin-bottom: 20px;
  font-size: 1.2rem;
  cursor: pointer;
}
i {
  font-size: 2rem;
  color: #efefef;
}
.destaq {
  font-weight: bold;
}
.inactive {
  display: none;
}
</style>
