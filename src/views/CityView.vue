<template>
  <div>
    <p>Page Is loading</p>
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <p>Loading ...</p>
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityView from "../components/AsyncCityView.vue";
</script>
