<template>
  <div class="p-10 py-8">
    <h1 class="text-lg font-light text-gray-900 mb-4">
      <LogoComp />
    </h1>
    <div class="text-sm text-gray-500 my-1 font-light">
      Data Source
      <a
        class="text-gray-700 hover:text-gray-900"
        href="https://github.com/ElaWorkshop/awesome-cn-cafe"
        target="_blank"
        rel="noreferrer"
        >Awesome CN Café</a
      >
    </div>
    <div class="text-sm text-gray-500 my-1 font-light">
      Updated <span class="text-gray-700">{{ fromNow(BuildTime) }}</span>
    </div>
    <div class="text-sm text-gray-500 my-1 font-light">
      Made by
      <a
        class="text-gray-700 hover:text-gray-900"
        href="https://github.com/antfu"
        target="_blank"
        rel="noreferrer"
        >@antfu</a
      >
    </div>
    <div class="text-sm text-gray-500 my-1 font-light">
      Source code on
      <a
        class="text-gray-700 hover:text-gray-900"
        href="hhttps://github.com/antfu/awesome-cn-cafe-web"
        target="_blank"
        rel="noreferrer"
        >Github</a
      >
    </div>
    <div v-if="loc" class="text-sm text-gray-500 my-1 font-light">
      My Location
      <span class="text-gray-700 ml-1">
        {{ loc.map((i) => +i.toFixed(5)).join(", ") }}
      </span>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { BuildTime } from "@/build";
import { fromNow } from "@/utils/time";
import { loc } from "../stores/store";
import LogoComp from "./LogoComp.vue";
</script>
