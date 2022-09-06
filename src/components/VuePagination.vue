<script setup lang="ts">
import { computed, ref } from "vue";

const props = defineProps<{
  totalPage: number;
  currentPage: number;
}>();

const pageData = computed(() => {
  let result: any = [];
  const current = props.currentPage;
  const total = props.totalPage;
  if (props.currentPage <= 5) {
    result = [1, 2, 3, 4, 5, "...", total];
  } else if (total - 4 <= current && current <= total) {
    result = [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  } else {
    result = [1, "...", current - 1, current, current + 1, "...", total];
  }
  return result;
});
</script>

<template>
  <div class="pagination-wrapper">
    <slot name="prev-button"></slot>
    <span v-for="i in pageData" :key="i">
      <slot :count="i"></slot>
    </span>
    <slot name="next-button"></slot>
  </div>
</template>

<style scoped>
.pagination-wrapper {
  align-items: center;
  display: flex;
  gap: 2px;
  justify-content: center;
  margin-top: 74px;
}

@media (min-width: 576px) {
  .pagination-wrapper {
    gap: 8px;
  }
}
</style>
