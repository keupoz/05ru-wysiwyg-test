<script setup lang="ts">
const props = defineProps({
  title: String,
  flat: Boolean,
});

const emit = defineEmits(["click"]);
</script>

<template>
  <button
    class="btn"
    :class="{ 'btn-flat': props.flat }"
    :title="props.title"
    @click="() => emit('click')"
  >
    <slot />
  </button>
</template>
