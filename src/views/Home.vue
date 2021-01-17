<template>
  <div class="flex justify-center w-full">
    <input
      type="text"
      placeholder="Enter Pokemon here"
      class="p-2 mt-10 border-2 border-blue-500"
      v-model="text"
    />
  </div>
  <div class="flex flex-wrap justify-center p-4 mt-10">
    <div
      class="ml-4 text-2xl text-blue-200"
      v-for="(pokemon, idx) in filteredPokemon"
      :key="idx"
    >
      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>
</template>

<script>
  import { computed, reactive, toRefs } from 'vue';

  export default {
    name: 'Home',
    setup() {
      const state = reactive({
        pokemons: [],
        urlIdLookup: {},
        text: '',
        filteredPokemon: computed(() => updatePokemon()),
      });

      const updatePokemon = () => {
        if (!state.text) {
          return [];
        }
        return state.pokemons.filter((pokemon) =>
          pokemon.name.includes(state.text)
        );
      };

      fetch('https://pokeapi.co/api/v2/pokemon?offset=0')
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          state.pokemons = data.results;
          state.urlIdLookup = data.results.reduce(
            (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
            {}
          );
        });

      return { ...toRefs(state) };
    },
  };
</script>
