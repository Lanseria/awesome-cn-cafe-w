<template>
  <div class="p-6">
    <h1 class="text-lg">
      {{ name }}
    </h1>
    <p class="text-gray-500 text-sm">
      <a
        :href="speedtest"
        :style="{ color }"
        class="inline-block"
        target="_blank"
        rel="noreferrer"
        >{{ speed }}</a
      >
      <template v-if="distance">
        <span class="inline-block align-middle mx-1">・</span>
        <span class="inline-block align-middle">{{ distance }}</span>
      </template>
      <span class="inline-block align-middle mx-1">・</span>
      <span class="inline-block align-middle">{{
        coordinates.map((i) => i.toFixed(3)).join(", ")
      }}</span>
    </p>
    <table class="px-2 mt-5 mb-3 text-sm">
      <tbody>
        <tr v-for="[key, value] in table" :key="key">
          <td class="pr-5 text-right text-gray-600 py-2">{{ key }}</td>
          <td class="py-2">{{ value }}</td>
        </tr>
      </tbody>
    </table>
    <div class="text-center mt-5 mb-2">
      <a
        :href="`https://uri.amap.com/marker?position=${location1}&name=${name}`"
        target="_blank"
        rel="noreferrer"
        class="border border-gray-200 rounded px-4 py-2 text-gray-600 text-sm mx-1"
      >
        高德地图
      </a>
      <a
        :href="`http://api.map.baidu.com/marker?location=${location2}&title=${name}&content=${AppName}&output=html`"
        target="_blank"
        rel="noreferrer"
        class="border border-gray-200 rounded px-4 py-2 text-gray-600 text-sm mx-1"
      >
        百度地图
      </a>
    </div>
  </div>
</template>
<script lang="ts" setup>
import type { CafeShop } from "@/stores/types";
import type { PropType } from "vue";
import { AppName } from "../constants";
import { parseShop } from "../utils/parseShop";
const props = defineProps({
  shop: {
    type: Object as PropType<CafeShop>,
    required: true,
  },
});

const {
  name,
  color,
  speed,
  speedtest,
  location1,
  location2,
  table,
  coordinates,
  distance,
} = parseShop(props.shop);
</script>
