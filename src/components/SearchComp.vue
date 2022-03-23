<template>
  <div
    class="h-screen w-screen relative grid"
    :style="{
      gridTemplateRows: 'max-content auto',
    }"
  >
    <div class="px-4 border-b border-gray-200">
      <input
        ref="inputRef"
        :value="searchString"
        @change="
          (e) => {
            searchString = e.target.value;
          }
        "
        placeholder="Search"
        class="px-3 py-4 outline-none w-full"
      />
      <Icon
        class="absolute top-0 right-0 m-4 text-2xl text-gray-600 hover:text-gray-800"
        icon="carbon:close"
        @click="() => setSearchOpen(false)"
      ></Icon>
    </div>
    <div class="overflow-auto py-2 px-6">
      <template v-if="searchString">
        <template v-if="searchResult.length">
          <ListItem
            v-for="item in searchResult"
            :key="item.coordinates.toString()"
            :shop="item"
          ></ListItem>
        </template>
        <template v-else>
          <div class="p-3 text-sm text-gray-400 text-center">无结果</div>
        </template>
      </template>
      <template v-else>
        <div class="p-3 text-sm text-gray-400 text-center">
          输入关键字以开始搜索
        </div>
      </template>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { Icon } from "@iconify/vue";
import ListItem from "./ListItem.vue";
import { searchString, setSearchOpen, searchResult } from "@/stores/store";
</script>
