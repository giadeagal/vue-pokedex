<template>
  <!--Header-->
  <header class="bg-slate-800 py-3 sticky top-0">
    <!--Primary navigation & Filters-->
    <nav class="mx-auto w-4/5 flex justify-start items-center py-2">
      <!-- Toggle all pokemon template-->
      <a
        class="text-white font-bold uppercase text-2xl mr-5 hover:text-emerald-500 cursor-pointer transition-all"
        @click="showAll"
      >
        Pokedex
      </a>

      <div class="flex flex-grow mr-16 justify-between items-center">
        <ul class="flex flex-row mt-1">
          <!-- Toggle caught pokemon template-->
          <li
            class="px-2 mr-5 text-white hover:text-emerald-500 cursor-pointer transition-all"
            @click="showTeam"
          >
            <a href="#">Captured pokemon</a>
          </li>

          <!-- Toggle missing pokemon template-->
          <li
            class="px-2 text-white hover:text-emerald-500 cursor-pointer transition-all"
            @click="showMissing"
          >
            <a href="#">Missing pokemon</a>
          </li>
        </ul>

        <!--Search bar-->
        <div>
          <div class="relative">
            <div
              class="flex absolute inset-y-0 left-0 items-center pl-3 pointer-events-none"
            >
              <svg
                aria-hidden="true"
                class="w-5 h-5 text-gray-500 dark:text-gray-400"
                fill="none"
                stroke="currentColor"
                viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
                ></path>
              </svg>
            </div>
            <input
              type="search"
              id="search"
              class="block p-4 pl-10 w-full text-sm text-gray-900 bg-gray-50 rounded-lg border border-gray-300 focus:ring-emerald-500 focus:border-emerald-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-emerald-500 dark:focus:border-emerald-500"
              placeholder="Search by name"
              @keyup.enter.prevent="search($event)"
            />
          </div>
        </div>
      </div>
    </nav>
  </header>

  <!--Main content-->
  <main class="w-2/3 mx-auto grid grid-cols-4 gap-x-0.5 gap-y-2">
    <!--This conditional template renders all pokemon-->
    <template v-if="flags.showAll === true">
      <!--PokeCard-->
      <a
        href="#"
        class="pokecard inline-block bg-slate-500 hover:bg-slate-300 m-4 p-5 rounded cursor-pointer flex justify-center items-center"
        v-for="pokemon in pokedex.listAll"
        :key="pokemon.name"
        @click="details(pokemon)"
      >
        <div class="name">
          <h2>{{ pokemon.name }}</h2>
          <img
            src="/pokeball-icon.png"
            alt=""
            class="inline-block w-3 ml-3"
            v-if="pokedex.listTeam.includes(pokemon)"
          />
        </div>
        <img :src="pokemon.img" alt="" />
      </a>
    </template>

    <!-- This conditional template renders all caught pokemon-->
    <template v-else-if="flags.showTeam === true">
      <a
        href="#"
        class="pokecard inline-block bg-slate-500 hover:bg-slate-300 m-4 p-5 rounded cursor-pointer flex justify-center items-center"
        v-for="pokemon in pokedex.listTeam"
        :key="pokemon.name"
        @click="details(pokemon)"
      >
        <div class="name">
          <h2>{{ pokemon.name }}</h2>
          <img
            src="../public/pokeball-icon.png"
            class="inline-block w-3 ml-3"
            alt=""
            v-if="pokedex.listTeam.includes(pokemon)"
          />
        </div>
        <img :src="pokemon.img" alt="" />

        <div
          class="button"
          v-if="!pokedex.listTeam.includes(pokemon)"
          @click="catchPokemon(pokemon)"
        >
          <img src="/pokeball.png" alt="" />
        </div>
      </a>
    </template>

    <!-- This conditional template renders all missing pokemon-->
    <template v-else-if="flags.showMissing === true">
      <a
        href="#"
        class="pokecard inline-block bg-slate-500 hover:bg-slate-300 m-4 p-5 rounded cursor-pointer flex justify-center items-center"
        v-for="pokemon in pokedex.listMissing"
        :key="pokemon.name"
        @click="details(pokemon)"
      >
        <div class="name">
          <h2>{{ pokemon.name }}</h2>
          <img
            src="/pokeball-icon.png"
            class="inline-block w-3 ml-3"
            alt=""
            v-if="pokedex.listTeam.includes(pokemon)"
          />
        </div>
        <img :src="pokemon.img" alt="" />
      </a>
    </template>
  </main>
  <!--This conditional template renders the details of the selected pokemon-->
  <template v-if="flags.showDetails === true">
    <div
      class="pokeDetails w-3/5 mx-auto p-20 flex justify-between"
      v-for="pokemon in pokedex.details"
      :key="pokemon.name"
    >
      <div class="details__left flex align-center w-1/3">
        <img :src="pokemon.big_img" alt="" class="max-w-xs" />
      </div>

      <div class="details__right ml-10 w-1/3">
        <p class="text-slate-400">#{{ pokemon.id }}</p>
        <h2 class="text-2xl text-emerald-400 display-inline my-3 font-bold">
          {{ pokemon.name }}
        </h2>

        <span
          v-if="this.pokedex.listTeam.includes(pokemon)"
          class="bg-green-100 text-green-800 text-sm font-medium px-2.5 py-0.5 my-3 rounded dark:bg-green-200 dark:text-green-900"
          >&check; Captured</span
        ><span
          v-else
          class="bg-red-100 text-red-800 text-sm font-medium mr-2 px-2.5 py-0.5 rounded dark:bg-red-200 dark:text-red-900"
          >&cross; Missing</span
        >

        <div
          class="mt-3 text-white text-sm"
          v-for="stat in pokemon.stats"
          :key="stat.name"
        >
          <span class="text-gray-400 font-semibold">{{ stat.name }}</span>
          <span>{{ ": " + stat.points }}</span>
        </div>
        <!--Back to pokedex button-->
        <button
          type="button"
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-2 py-1 my-3 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
          @click="showAll"
        >
          &leftarrow; Pok&eacute;dex
        </button>

        <!--Catch this pokemon button-->
        <button
          type="button"
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm ml-3 px-2 py-1 mt-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800"
          v-if="!pokedex.listTeam.includes(pokemon)"
          @click="catchPokemon(pokemon)"
        >
          catch it!
        </button>
      </div>
    </div>
    <p class="description text-center text-slate-300 font-semibold">
      {{ flavorText }}
    </p>
  </template>

  <!--Footer-->
  <footer class="bg-slate-800 py-2 w-full fixed bottom-0 flex justify-center">
    <p class="text-slate-400">
      Coded with <span class="text-emerald-500">&hearts;</span> by
      <a class="hover:text-emerald-500" href="https://github.com/giadeagal"
        >Gianluca De Angelis</a
      >
    </p>
  </footer>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      pokemonCount: 898,
      input: "",
      flavorText: "",
      pokedex: {
        listAll: [],
        listTeam: [],
        listMissing: [],
        details: [],
      },
      flags: {
        showAll: true,
        showTeam: false,
        showMissing: false,
        showDetails: false,
      },
    };
  },
  methods: {
    catchPokemon(pokemon) {
      this.pokedex.listTeam.push(pokemon);

      const index = this.pokedex.listMissing.indexOf(pokemon);
      this.pokedex.listMissing.splice(index, 1);
    },
    showAll() {
      this.flags["showAll"] = true;
      this.flags["showTeam"] = false;
      this.flags["showMissing"] = false;
      this.flags["showDetails"] = false;
    },
    showTeam() {
      this.flags["showAll"] = false;
      this.flags["showTeam"] = true;
      this.flags["showMissing"] = false;
      this.flags["showDetails"] = false;
    },
    showMissing() {
      this.flags["showAll"] = false;
      this.flags["showTeam"] = false;
      this.flags["showMissing"] = true;
      this.flags["showDetails"] = false;
    },
    showSingle() {
      this.showDetails();
    },
    showDetails() {
      this.getDescription();
      this.flags["showAll"] = false;
      this.flags["showTeam"] = false;
      this.flags["showMissing"] = false;
      this.flags["showSingle"] = false;
      this.flags["showDetails"] = true;
    },
    normalizeQuery(query) {
      let normalization =
        query.charAt(0).toUpperCase() + query.toLowerCase().slice(1);
      return normalization;
    },
    search($event) {
      this.pokedex.details = [];

      let query = $event.target.value;
      let normalizedQuery = this.normalizeQuery(query);

      for (let i = 0; i < this.pokedex.listAll.length; i++) {
        if (this.pokedex.listAll[i].name.match(normalizedQuery)) {
          this.pokedex.details.push(this.pokedex.listAll[i]);
        }
      }

      this.showDetails();
    },
    details(pkm) {
      this.pokedex.details = [];
      this.pokedex.details.push(pkm);
      this.showDetails();
    },
    async getDescription() {
      let id = this.pokedex.details[0].id;
      let url = "https://pokeapi.co/api/v2/pokemon-species/" + id;

      await axios.get(url).then((descriptions) => {
        descriptions.data.flavor_text_entries.map((desc) => {
          if (desc.language.name === "en") {
            this.flavorText = desc.flavor_text;
          }
        });
      });
    },
  },
  async created() {
    for (let i = 1; i <= this.pokemonCount; i++) {
      let url = "https://pokeapi.co/api/v2/pokemon/" + i;

      await axios.get(url).then((r) => {
        let pokemon = {
          id: r.data.id,
          name: r.data.name.charAt(0).toUpperCase() + r.data.name.slice(1),
          img: r.data.sprites.front_default,
          big_img: r.data.sprites.other.dream_world.front_default,
          types: r.data.types.map((el) => {
            let type =
              el.type.name.charAt(0).toUpperCase() +
              el.type.name.toLowerCase().slice(1);
            return type;
          }),
          stats: [
            {
              name: "HP",
              points: r.data.stats[0].base_stat,
            },
            {
              name: "Attack",
              points: r.data.stats[1].base_stat,
            },
            {
              name: "Defense",
              points: r.data.stats[2].base_stat,
            },
            {
              name: "Special attack",
              points: r.data.stats[3].base_stat,
            },
            {
              name: "Special defense",
              points: r.data.stats[4].base_stat,
            },
            {
              name: "Speed",
              points: r.data.stats[5].base_stat,
            },
          ],
        };
        this.pokedex.listAll.push(pokemon);
        this.pokedex.listMissing.push(pokemon);
      });
    }
  },
};
</script>

<style>
.pokecard {
  transition: all 0.2s;
}
</style>
