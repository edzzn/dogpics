<script lang="ts">
import LoadingSpinner from "./LoadingSpinner.vue";

export default {
  components: {
    LoadingSpinner,
  },
  data: () => ({
    imageUrl: "https://via.placeholder.com/300",
    hasError: false,
    isLoading: false,
  }),

  methods: {
    async fetchData() {
      this.isLoading = true;
      const url = "https://dog.ceo/api/breeds/image/random";
      const response = await (await fetch(url)).json();

      if (response.status === "success") {
        this.imageUrl = response.message;
        this.hasError = false;
        this.isLoading = false;
      } else {
        this.hasError = true;
        this.isLoading = false;
      }
    },
  },
  created() {
    this.fetchData();
  },
};
</script>

<template>
  <div class="max-w-sm rounded overflow-hidden shadow-lg bg-white">
    <div v-if="isLoading" class="h-80 w-80 flex items-center justify-center">
      <LoadingSpinner />
    </div>
    <div
      v-else-if="hasError"
      class="bg-red-300 h-40 w-80 text-xl flex items-center justify-center"
    >
      Please try again
    </div>
    <img v-else="!hasError" :src="imageUrl" alt="Cute dog" />

    <div class="p-6">
      <button
        class="rounded bg-cyan-600 text-white w-full p-3"
        @click="fetchData"
      >
        Change image
      </button>
    </div>
  </div>
</template>

<style scoped></style>
