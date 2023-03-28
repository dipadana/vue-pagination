<script setup lang="ts">
import { ref } from "vue";
import Pagination from "./components/VuePagination.vue";

const currentPage = ref(1);
const totalPage = ref(15);
const maxPage = ref(6);

function changePage(param: string | number) {
  if (param === "+" && currentPage.value < totalPage.value) {
    currentPage.value++;
  } else if (param === "-" && currentPage.value > 1) {
    currentPage.value--;
  } else if (param !== "..." && param !== "+" && param !== "-") {
    currentPage.value = Number(param);
  }
}
</script>

<template>
  <div>
    <pagination
      :total-page="totalPage"
      :current-page="currentPage"
      :max-page="maxPage"
    >
      <template v-slot:prev-button>
        <button @click="changePage('-')">{{ "<" }}</button>
      </template>
      <template #default="{ count }">
        <button
          @click="changePage(count)"
          :class="currentPage == count ? 'text-bold' : ''"
        >
          {{ count }}
        </button>
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

button {
  background-color: white;
  border: none;
  cursor: pointer;
}
</style>
