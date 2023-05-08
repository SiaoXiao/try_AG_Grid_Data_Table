<template>
  <div class="container">
    <AGdataTable />
  </div>

  <span class="copyright">{{ nowDay }} Keith</span>
</template>

<script>
import { ref } from "@vue/reactivity";
import AGdataTable from "./components/AGdataTable.vue";
export default {
  components: {
    AGdataTable,
  },
  setup() {
    const nowDay = ref("");

    const timeFormate = () => {
      let newDate = new Date();
      let year = newDate.getFullYear();
      let month =
        newDate.getMonth() + 1 < 10
          ? "0" + (newDate.getMonth() + 1)
          : newDate.getMonth() + 1;
      let date =
        newDate.getDate() < 10 ? "0" + newDate.getDate() : newDate.getDate();
      nowDay.value = year + "年" + month + "月" + date + "日";
    };

    timeFormate();

    return {
      nowDay,
      timeFormate,
    };
  },
};
</script>