<template>
  <section id="cards-container" class="card-container">
    <h2>Cards Container</h2>
    <ul id="pokemon">
      <div class="card" v-for="onePokemon in pokemon" :key="onePokemon.id">
        <h3>{{ onePokemon.name }}</h3>
        <img :src="onePokemon.sprites.front_default" />
        <button @click="this.$emit(deletePokemon)">X</button>
      </div>
    </ul>
  </section>
</template>

<script>
export default {
  name: "CardsContainer",
  props: {
    pokemon: Array,
    deletePokemon: Function
  }
};
</script>

<style scoped>
.card-container {
  border: 1px solid black;
}

#pokemon {
  display: flex;
  flex-wrap: wrap;
}

.card {
  border: 1px solid black;
}
</style>
