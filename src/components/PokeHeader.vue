<template>
  <header id="header" class="bg-gray-700 text-slate-300">
    <nav class="mx-auto w-4/5 flex justify-start items-center py-2">
      <h1
        class="text-white font-bold uppercase text-2xl mr-5 hover:text-emerald-500 cursor-pointer transition-all"
      >
        Pokedex
      </h1>

      <div class="flex flex-grow mr-16 justify-between items-center">
        <ul class="flex flex-row mt-1">
          <li
            class="px-2 mr-5 text-white hover:text-emerald-500 cursor-pointer transition-all"
          >
            Captured pokemon
          </li>
          <li
            class="px-2 text-white hover:text-emerald-500 cursor-pointer transition-all"
          >
            Missing pokemon
          </li>
        </ul>

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
            />
          </div>
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  name: "PokeHeader",
};
</script>
