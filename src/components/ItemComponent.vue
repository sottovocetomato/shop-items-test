<template>
  <div class="item">
    {{ itemName }}
  </div>
</template>

<script setup>
const props = defineProps({
  itemName: String,
});
</script>

<style scoped></style>
