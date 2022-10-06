<template>
  <PokeHeader />

  <div
    class="container mx-auto mt-10 w-1/2 p-20 max-h-[25rem] overflow-auto bg-slate-800 grid grid-cols-3 gap-3"
  >
    <pokeCard :pokedex="pokedex" />
  </div>
  <PokeFooter />
</template>

<script>
import PokeHeader from "@/components/PokeHeader.vue";
import PokeCard from "./components/PokeCard.vue";
import PokeFooter from "./components/PokeFooter.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    PokeHeader,
    PokeCard,
    PokeFooter,
  },
  data() {
    return {
      pokemonCount: 898,
      pokedex: {
        listAll: [],
        listTeam: [],
        listMissing: [],
      },
    };
  },
  async created() {
    for (let i = 1; i <= this.pokemonCount; i++) {
      let url = "https://pokeapi.co/api/v2/pokemon/" + i;

      await axios.get(url).then((r) => {
        let pokemon = {
          name: r.data.name.charAt(0).toUpperCase() + r.data.name.slice(1),
          img: r.data.sprites.front_default,
          types: r.data.types.map((el) => {
            let type =
              el.type.name.charAt(0).toUpperCase() +
              el.type.name.toLowerCase().slice(1);
            return type;
          }),
        };
        this.pokedex.listAll.push(pokemon);
        this.pokedex.listMissing.push(pokemon);
      });
    }
  },
};
</script>

<style></style>
