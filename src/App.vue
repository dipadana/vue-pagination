<script setup lang="ts">
import { ref } from "vue";
import Pagination from "./components/VuePagination.vue";

const totalPage = ref(20);
const currentPage = ref(1);

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
    <pagination :total-page="totalPage" :current-page="currentPage">
      <template v-slot:prev-button>
        <button @click="changePage('-')">{{ "<" }}</button>
      </template>
      <template #default="{ count }">
        <p :class="currentPage == count ? 'text-bold' : ''">{{ count }}</p>
      </template>
      <template v-slot:next-button>
        <button @click="changePage('+')">{{ ">" }}</button>
      </template>
    </pagination>
  </div>
</template>

<style scoped>
.text-bold {
  font-weight: bold;
}
</style>
