<script setup lang="ts">
import NavComp from "@/components/NavComp.vue";
import MapComp from "@/components/MapComp.vue";
import FloatControl from "@/components/FloatControl.vue";
import ModalComp from "@/components/ModalComp.vue";
import DetailComp from "@/components/DetailComp.vue";
import SearchComp from "@/components/SearchComp.vue";
import AboutComp from "@/components/AboutComp.vue";
import {
  city,
  data,
  filteredGeo,
  filter,
  setCurrent,
  current,
  about,
  setAbout,
  searchOpen,
  setSearchOpen,
} from "./stores/store";
</script>

<template>
  <div class="h-full relative">
    <div
      class="h-full grid relative"
      style="grid-template-rows: max-content auto"
    >
      <NavComp />
      <MapComp
        :city="city"
        :data="data"
        :geo="filteredGeo"
        :filter="filter"
      ></MapComp>
    </div>
    <FloatControl />
    <ModalComp :value="!!current" :setValue="() => setCurrent(null)">
      <DetailComp v-if="current" :shop="current"></DetailComp>
    </ModalComp>
    <ModalComp :value="about" :setValue="() => setAbout(false)">
      <AboutComp />
    </ModalComp>
    <ModalComp :value="searchOpen" :setValue="() => setSearchOpen(false)">
      <SearchComp />
    </ModalComp>
  </div>
</template>
