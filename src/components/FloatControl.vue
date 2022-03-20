<template>
  <div class="fixed bottom-0 left-0 mx-3 mb-10">
    <div class="bg-white rounded shadow flex flex-col p-2">
      <button
        v-for="color in Colors"
        :key="color"
        class="py-2 px-1 focus:outline-none outline-none transition-all duration-300"
        :class="filter === color || filter === 'all' ? '' : 'opacity-25'"
        :style="{
          color,
        }"
        @click="() => setFilter(color)"
      >
        <Icon class="pointer-events-none" :icon="ColorToIcon[color]" />
      </button>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { Icon } from "@iconify/vue";
import { filter, setFilter } from "@/stores/store";
import { Colors, ColorToIcon } from "../constants";
</script>
