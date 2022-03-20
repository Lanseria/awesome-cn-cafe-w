<template>
  <div
    class="fixed top-0 bottom-0 left-0 right-0 z-40"
    :class="value ? '' : 'pointer-events-none'"
  >
    <div
      class="bg-white bg-opacity-85 bottom-0 left-0 right-0 top-0 absolute transition-opacity duration-500 ease-out"
      :class="value ? 'opacity-75' : 'opacity-0'"
      @click="() => setValue(false)"
    ></div>
    <div
      class="bg-white absolute transition-all duration-200 ease-out border-gray-200 bottom-0 left-0 right-0 border-t"
      :style="value ? {} : { transform: 'translateY(100%)' }"
    >
      <slot></slot>
    </div>
  </div>
</template>
<script lang="ts" setup>
defineProps({
  value: {
    type: Boolean,
    required: true,
  },
  setValue: {
    type: Function,
    required: true,
  },
});
</script>
