<script setup lang="ts">
import { computed, ref } from "vue";

const props = defineProps<{
  totalPage: number;
  currentPage: number;
}>();

// const totalPage = ref(props.totalPage);
// const currentPage = ref(props.currentPage);

// const totalPage = ref(10);
// const currentPage = ref(1);

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
  <div>
    <slot name="prev-button"></slot>
    <span v-for="i in pageData" :key="i">
      <p>
        <slot :count="i"></slot>
      </p>
    </span>
    <slot name="next-button"></slot>
  </div>
</template>

<style scoped>
div {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 200px;
}

p {
  font-size: 50px;
  margin: 0 40px;
}
</style>
