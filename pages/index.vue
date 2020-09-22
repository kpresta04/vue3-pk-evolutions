<template>
  <div class="wrapper">
    <pokemonCards :pokemons="starters" :selectedId="selectedId" @pokemonClicked="fetchEvolutions" />

    <pokemonCards :pokemons="evolutions" />
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import pokemonCards from "../components/pokemonCards.vue";
import Card from "../components/card.vue";

const api = "https://pokeapi.co/api/v2/pokemon";
const ids = [1, 4, 7];
interface Starter {
  id?: number;
  name?: string;
  sprite?: string;
  types?: string[];
}

export default Vue.extend({
  components: {
    pokemonCards,
  },
  data() {
    return {
      starters: [] as any,
      evolutions: [] as any,
      selectedId: null as any,
    };
  },
  async mounted() {
    const result = await this.fetchData(ids);

    this.starters = result;
  },
  methods: {
    async fetchEvolutions(pokemon: any) {
      this.selectedId = pokemon.id;
      this.evolutions = await this.fetchData([pokemon.id + 1, pokemon.id + 2]);
    },
    async fetchData(ids: number[]) {
      const responses = await Promise.all(
        ids.map((id) => window.fetch(`${api}/${id}`))
      );
      const data = await Promise.all(responses.map((res) => res.json()));
      const result = data.map((datum) => ({
        id: datum.id,
        name: datum.name,
        sprite: datum.sprites.other["official-artwork"].front_default,
        types: datum.types.map((type: any) => type.type.name),
      }));
      return result;
    },
  },
});
</script>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>