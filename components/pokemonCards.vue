<template>
  <div class="cards">
    <Card
      v-for="pokemon in pokemons"
      @click.native="click(pokemon)"
      :class="{ opace: selectedId && pokemon.id !== selectedId }"
      :key="pokemon.index"
    >
      <template v-slot:title>{{ pokemon.name }} #{{ pokemon.id }}</template>

      <template v-slot:content>
        <img :src="pokemon.sprite" />
      </template>

      <template v-slot:description>
        <div v-for="type in pokemon.types" :key="type.index">{{ type }}</div>
      </template>
    </Card>
  </div>
</template>

<script>
import Card from "../components/card.vue";
export default {
  components: {
    Card,
  },
  props: {
    selectedId: {
      type: Number,
    },
    pokemons: {
      type: Array,
    },
  },
  methods: {
    click(pokemon) {
      this.$emit("pokemonClicked", pokemon);
    },
  },
};
</script>

<style scoped>
.cards {
  display: flex;
  margin: 20px 0;
}
.opace {
  opacity: 0.5;
}
.card:hover {
  opacity: 1;
}
img {
  width: 100%;
}
</style>