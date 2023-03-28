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
    <div class="pagination-wrapper">
      <pagination
        :current-page="currentPage"
        :total-page="totalPage"
        :max-page="maxPage"
      >
        <template v-slot:prev-button>
          <button @click="changePage('-')">{{ "<" }}</button>
        </template>
        <template #default="{ pagination }">
          <button
            v-for="i in pagination"
            :key="i"
            :class="currentPage == i ? 'text-bold' : ''"
            @click="changePage(i)"
          >
            {{ i }}
          </button>
        </template>
        <template v-slot:next-button>
          <button @click="changePage('+')">{{ ">" }}</button>
        </template>
      </pagination>
    </div>
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
