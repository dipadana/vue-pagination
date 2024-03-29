<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  modelValue: number;
  totalPage: number;
  maxPage: number;
}>();

const emit = defineEmits(["update:modelValue"]);

function paginationLogic(max: number, current: number, total: number) {
  let result: Array<string | number> = [];
  let temp: Array<string | number> = [];

  if (total <= max) {
    for (let i = 1; i <= total; i++) result.push(i);
  } else if (current <= max - 2) {
    for (let i = 1; i <= max - 2; i++) temp.push(i);
    result = [...temp, "...", total];
  } else if (total - (max - 3) <= current && current <= total) {
    temp = [];
    for (let i = 0; i <= max - 3; i++) temp.unshift(total - i);
    result = [1, "...", ...temp];
  } else {
    temp = [current];
    const loop = (max - 4) / 2;
    for (let i = 1; i <= loop; i++) temp.unshift(current - i);
    for (let i = 1; i <= loop; i++) temp.push(current + i);
    result = [1, "...", ...temp, "...", total];
  }

  return result;
}

const pageData = computed(() => {
  return paginationLogic(props.maxPage, props.modelValue, props.totalPage);
});

function changePage(param: string | number) {
  if (param === "+" && props.modelValue < props.totalPage) {
    emit("update:modelValue", props.modelValue + 1);
  } else if (param === "-" && props.modelValue > 1) {
    emit("update:modelValue", props.modelValue - 1);
  } else if (param !== "..." && param !== "+" && param !== "-") {
    emit("update:modelValue", Number(param));
  }
}
</script>

<template>
  <slot name="prev-button" :change-page="changePage"></slot>
  <slot :pagination="pageData" :change-page="changePage"></slot>
  <slot name="next-button" :change-page="changePage"></slot>
</template>
