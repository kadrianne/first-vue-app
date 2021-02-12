<template>
  <div id="app">
    <h1 :class="this.titleColor">{{ this.title }}</h1>
    <button @click="handleClick">Toggle Title Color</button>
    <CardsContainer @delete-pokemon="deletePokemon" :pokemon="this.pokemon" />
  </div>
</template>

<script>
import CardsContainer from "./components/CardsContainer";

export default {
  name: "App",
  mounted: function() {
    this.$nextTick(function() {
      fetch("https://pokeapi.co/api/v2/pokemon/?limit=150")
        .then(response => response.json())
        .then(data => {
          const allPokemon = data.results.map((pokemon, index) => {
            const id = index + 1;
            return fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
          });
          Promise.all(allPokemon)
            .then(response =>
              Promise.all(response.map(pokeResult => pokeResult.json()))
            )
            .then(pokemon => (this.pokemon = pokemon));
        });
    });
  },
  data() {
    return {
      title: "Pokemon App",
      titleColor: "red",
      pokemon: []
    };
  },
  components: {
    CardsContainer
  },
  computed: {
    titleLength() {
      return this.title.length;
    }
  },
  methods: {
    handleClick() {
      this.titleColor == "red"
        ? (this.titleColor = "blue")
        : (this.titleColor = "red");
    },
    deletePokemon(event, id) {
      console.log(id, event);
    }
  }
};
</script>

<style>
.red {
  color: "red";
}

.blue {
  color: "blue";
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
