<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  totalPage: number;
  currentPage: number;
  maxPage: number;
}>();

function paginationLogic(max: number, current: number, total: number) {
  let result: Array<string | number> = [];
  let temp: Array<string | number> = [];

  if (total <= max) {
    for (let i = 1; i <= total; i++) result.push(i);
  } else if (props.currentPage <= max - 2) {
    for (let i = 1; i <= max - 2; i++) temp.push(i);
    result = [...temp, "...", total];
  } else if (total - (max - 3) <= current && current <= total) {
    temp = [];
    for (let i = 0; i <= max - 3; i++) temp.unshift(total - i);
    result = [1, "...", ...temp];
  } else {
    if (max >= 10) {
      temp = [current];
      const loop = (max - 4) / 2;
      for (let i = 1; i <= loop; i++) temp.unshift(current - i);
      for (let i = 1; i <= loop; i++) temp.push(current + i);
      result = [1, "...", ...temp, "...", total];
    } else if (max >= 6 && max <= 9) {
      result = [1, "...", current - 1, current, current + 1, "...", total];
    } else {
      result = [1, "...", current, "...", total];
    }
  }

  return result;
}

const pageData = computed(() => {
  return paginationLogic(props.maxPage, props.currentPage, props.totalPage);
});
</script>

<template>
  <div class="pagination-wrapper">
    <slot name="prev-button"></slot>
    <template v-for="i in pageData" :key="i">
      <slot :count="i"></slot>
    </template>
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
