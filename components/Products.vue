<script setup>
const state = reactive({
  data: null,
  isLoading: true,
});

const searchTerm = ref("");

onMounted(async () => {
  try {
    const response = await $fetch("https://fakestoreapi.com/products");
    state.data = response;
  } catch (error) {
    console.error(error);
  } finally {
    state.isLoading = false;
  }
});

const filteredData = computed(() =>
  state.data.filter((item) =>
    item.title.toLowerCase().includes(searchTerm.value.toLowerCase())
  )
);
</script>
<template>
  <form class="flex items-center max-w-sm mx-auto">
    <label for="simple-search" class="sr-only">Search</label>
    <div class="relative w-full">
      <div
        class="absolute inset-y-0 flex items-center pointer-events-none start-0 ps-3"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="1em"
          height="1em"
          viewBox="0 0 24 24"
        >
          <path
            fill="white"
            d="m19.6 21l-6.3-6.3q-.75.6-1.725.95T9.5 16q-2.725 0-4.612-1.888T3 9.5t1.888-4.612T9.5 3t4.613 1.888T16 9.5q0 1.1-.35 2.075T14.7 13.3l6.3 6.3zM9.5 14q1.875 0 3.188-1.312T14 9.5t-1.312-3.187T9.5 5T6.313 6.313T5 9.5t1.313 3.188T9.5 14"
          />
        </svg>
      </div>
      <input
        v-model="searchTerm"
        type="text"
        id="simple-search"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full ps-10 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        placeholder="Search branch name..."
        required
      />
    </div>
  </form>

  <div
    v-if="state.isLoading"
    class="h-[80vh] flex gap-x-3 justify-center items-center font-bold text-2xl"
  >
    <div
      class="h-8 w-8 bg-white rounded-full animate-bounce [animation-delay:-0.3s]"
    ></div>
    <div
      class="h-8 w-8 bg-white rounded-full animate-bounce [animation-delay:-0.15s]"
    ></div>
    <div class="w-8 h-8 bg-white rounded-full animate-bounce"></div>
  </div>

  <div
    v-else
    class="grid max-w-5xl grid-cols-1 mx-auto mt-8 text-center lg:max-w-6xl gap-y-8 sm:gap-x-8 sm:grid-cols-2 lg:grid-cols-3 sm:mt-12 lg:mt-20 sm:text-left"
  >
    <div
      class="text-2xl font-semibold tracking-wide text-gray-400 capitalize"
      v-if="filteredData.length === 0"
    >
      No data found.
    </div>
    <template v-for="item in filteredData" :key="item.id">
      <a
        class="flex flex-col overflow-hidden transition bg-white border shadow-sm group rounded-xl hover:shadow-lg dark:bg-neutral-900 dark:border-neutral-700 dark:shadow-neutral-700/70"
        href="#"
        data-aos="fade-up"
        data-aos-duration="1000"
      >
        <div
          class="relative pt-[50%] sm:pt-[60%] lg:pt-[80%] rounded-t-xl overflow-hidden"
        >
          <img
            class="absolute top-0 object-fill transition-transform duration-500 ease-in-out size-full start-0 group-hover:scale-105 rounded-t-xl"
            :src="item.image"
            alt="Image Description"
          />
        </div>
        <div class="p-4 md:p-5">
          <h3 class="text-lg font-bold text-gray-800 truncate dark:text-white">
            {{ item.title }}
          </h3>
          <p class="mt-1 text-gray-500 dark:text-neutral-400 lg:line-clamp-4">
            {{ item.description }}
          </p>
          <p class="text-green-500 text-opacity-80">${{ item.price }}</p>
        </div>
      </a>
    </template>
  </div>
</template>
