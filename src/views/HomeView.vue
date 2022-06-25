<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div
      class="flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32 bg-red-500"
    >
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-whit text-center text-3xl pb-4">IP Address Tracker</h1>
        <div class="flex">
          <input
            class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none"
            type="text"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "HomeView",
  components: {},
};
</script>
