<template>
  <div
    class="text-center p-2 border-b border-gray-200 flex font-light relative"
  >
    <div
      class="text-sm text-gray-700 flex-none m-1 hover:bg-gray-100 rounded px-2 py-1 cursor-pointer relative"
    >
      {{ cityName }}
      <Icon
        icon="carbon:chevron-down"
        class="inline-block text-gray-500 align-middle"
      >
      </Icon>
      <select
        class="absolute top-0 bottom-0 left-0 right-0 opacity-0"
        :value="city"
        @change="(e) => changeCity(e!.target!.value )"
      >
        <option v-for="[k, v] in cities" :value="k" :key="k">
          {{ v.name }} ({{ v.count }})
        </option>
      </select>
    </div>
    <div class="flex-auto"></div>
    <div
      class="hover:bg-gray-100 hover:text-gray-700 text-gray-500 rounded p-2 mx-1 m-auto cursor-pointer"
      @click="() => setSearchOpen(true)"
    >
      <Icon icon="carbon:search" class="text-lg" />
    </div>
    <div
      class="hover:bg-gray-100 hover:text-gray-700 text-gray-500 rounded p-2 mx-1 m-auto cursor-pointer"
      @click="() => emitter.emit('track')"
    >
      <Icon icon="carbon:map" class="text-lg" />
    </div>

    <div
      class="text-lg text-gray-700 absolute -mr-2 select-none"
      :style="{
        top: '50%',
        left: '50%',
        transform: 'translate(-50%, -50%)',
      }"
      @click="() => setAbout(true)"
    >
      <LogoComp />
    </div>
  </div>
</template>
<script lang="ts" setup>
import { Icon } from "@iconify/vue";
import { emitter } from "../event";
import LogoComp from "@/components/LogoComp.vue";
import {
  city,
  cityName,
  cities,
  changeCity,
  setAbout,
  setSearchOpen,
} from "@/stores/store";
</script>
