<script setup lang="ts">
import { computed, ref } from "vue";

const totalPage = ref(20);
const currentPage = ref(1);

const pageData = computed(() => {
  let result: any = [];
  const current = currentPage.value;
  const total = totalPage.value;
  if (currentPage.value <= 5) {
    result = [1, 2, 3, 4, 5, "...", total];
  } else if (total - 4 <= current && current <= total) {
    result = [1, "...", total - 4, total - 3, total - 2, total - 1, total];
  } else {
    result = [1, "...", current - 1, current, current + 1, "...", total];
  }
  return result;
});

function changePage(param: string) {
  if (param === "+" && currentPage.value < totalPage.value) {
    currentPage.value++;
  } else if (param === "-" && currentPage.value > 1) {
    currentPage.value--;
  }
}
</script>

<template>
  <div>
    <button @click="changePage('-')">{{ "<" }}</button>
    <p
      v-for="i in pageData"
      :key="i"
      :class="currentPage == i ? 'text-bold' : ''"
    >
      {{ i }}
    </p>
    <button @click="changePage('+')">{{ ">" }}</button>
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
  font-size: 100px;
  margin: 0 80px;
}

.text-bold {
  font-weight: bold;
}
</style>
